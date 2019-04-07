---
title: "[Kaggle] Quora Insincere Questions Classification 참가 후기 (1)"
layout: post
headerImage: false
category: blog
author: huiwon
---
Quora Insincere Questions Classification에 참가한 후기를 늦게나마 정리한다. 글로 후기를 남기기로 마음먹었는데, 다른 일들로 우선순위가 계속 미뤄져 대회가 끝난지 두달 가량이 벌써 지나버렸다. 최근 비슷한 주제의 컴패티션으로 "Jigsaw Unintended Bias in Toxicity Classification"가 열린 걸 보고 오늘은 꼭 후기를 써야겠다고 느꼈다.  
Quora Insincere Questions Classification(이하 QIQC)는 내가 캐글에서 solo로 참가한 두번째 대회다. (Kaggle에 가입하여 대회에 등록해서 public kernel을 다뤄본 것을 제외한다면...) 처음으로 참가한 대회인 "Quick, Draw! Doodle Recognition Challenge"에서 많이 고군분투하다가 결국 메달을 따기 실패했었기 때문에 이번 대회는 꼭 메달을 따고 싶었다. vision관련 테스크는 내게 익숙하지 않은 주제였지만, "QIQC"는 내가 좀 더 익숙한 nlp관련 주제였기 때문에 좀 더 어려움이 덜하지 않을까 싶었다. 물론 생각지 못했던 난관들이 있어서 끝까지 긴장을 늦출 수 없었다.  
### 대회 개요  
QIQC는 Quora에서 주최한 대회다. Quora는 미국의 지식인같은 사이트로, 질문과 그에 대한 대답을 게시하는 플랫폼이다.(여담을 붙이자면, 답변의 퀄리티가 굉장히 좋은 편이다. 실제 교수, 연구자, 산업종사자의 답변을 심심치않게 볼 수 있다.) 이 competition의 목적은 question set이 주어지면 그 질문이 정말 진실됐는지(sincere) 판별하는 것이다. 여기서 정의하는 insincere한 질문이란 중립적이지 않거나, 누군가를 폄하하거나 비하하거나, 선동적이거나, 거짓에 기반한 내용이거나 성적인 내용을 포함하는 것을 말한다. 이러한 insincere한 질문일 경우룰 target을 1로 sincere할 경우 target을 0으로 예측해야 한다. Evaluation은 F1 Score를 사용한다.  
### 이 대회의 제한조건들
이 대회가 쉽지 않았던 이유를 정리하면 아래와 같다.
* kernel-only 2 stage competition : submission을 제출하는 방식이 아니라, 커널에서 코드를 돌려서 나온 결과물로 채점이 이루어진다. GPU kernel의 경우 2시간, CPU kernel의 경우 6시간 안에 결과가 나와야한다. (참고로 kaggle kernel에서 사용되는 GPU는 K80으로 상당히 느린편이다.) 따라서 해당시간안에 할 수 있는 내용이 매우 한정된다. 데이터 전처리부터, 모델 학습과 예측이 전부 이루어져야 하므로, 모델의 크기를 마음대로 늘릴 수도 없고, ensemble을 적용하기도 힘들다. 1 stage 에서는 최종 모델로 2개를 선택하여 제출하고, 2 stage에서 private test data에 관하여 모델이 돌아간다. private test data는 공개되지 않았던 전체 테스트 셋의 80%를 더 포함하므로 1 stage에서 테스트할 때보다 좀 더 여유롭게 시간이 남는 모델을 만들어야 한다. (2 stage에서 추가시간을 주지 않기때문에 2시간/6시간을 초과할 경우, 리더보드 순위에서 배제된다. 실제 이 competition은 4천명이 참가했지만, 1400여명만이 리더보드에서 보인다. 나머지는 대부분 TLE...ㅠㅠ)
<br/>
* External data sources are not allowed. : 최근 NLP의 추세가 large corpus에 대해 LM을 학습하고, task에 대해 다시 fine-tuning하는 방식인데, 이 대회에서는 외부데이터를 금지한다. 만약 금지하지 않았다면, bert변형들로 리더보드를 메꿨을 것 같긴하다. public kernel중에 bert를 사용하여 결과를 확인한 모델이 있긴하지만, 사실 이걸 제출하면 룰 위반이다.
<br/>
* Data noise: 주어진 데이터는 quora에서 사람이 annotate한 것으로 오류가 꽤 많이 남아있다. (주체측도 이를 미리 공지했다.) train data를 좀 살펴보면 잘못 표기된 데이터를 쉽게 찾을 수 있다. 예를 들면, "동양인은 서양인을 좋아하지 않아?"와 같은 누가 봐도 insincere한 질문이 0으로 태깅되어 있다. (반대의 경우도 존재.)
<br/>
* Imbalanced data : train data의 sincere question과 insincere한 question의 비율이 매우 불균형하다. 대략 9:1에서 8:2정도의 비율로 기억한다. 위의 데이터 노이즈와 더불어 학습을 어렵게 만든다. (external data를 금지하기에 다른 데이터셋을 추가할 수도 없다.)
<br/>
* CV score and LB Score are not correlate: validation set이 test set을 잘 대변할 수 있다고 가정한다면, 이상적으로 CV score와 LB score는 서로 연관성을 보여야한다. 즉 CV score가 이전보다 높게 나왔다면 LB score도 그러한 경향을 따라야한다. 그런데 나뿐만 아니라 대부분의 참가자들이 서로 다른 CV, LB score의 간극에 고통을 겼었다. 보통 이러한 경우 가능한 가설로 첫째는 public test data set이 모델을 평가하기 너무 작을 경우이며, 둘째는 train data와 test data의 distribution이 다를 경우이다.
<br/>
* Unstable: 같은 kernel을 제출하였음에도 scoreboard의 결과가 꽤 차이가 날 수 있음이 보고되었다. 즉 같은 모델을 제출해도 public scoreboard의 등수를 재현하지 못하는 경우들이 생겼다. seed값에 따라도 결과가 뒤바뀌는 경우가 생겨서, 좋은 seed를 찾겠다는 참가자도 있었지만 이건 좋은 approach는 아닌 것 같다.  
### 내가 시도해 본 방법  
위와 같은 이유로 마의 벽 0.7에서 다들 고군분투하였다. 이러한 제한 조건안에서 내가 시도해본 내용들을 적어보자면, 다음과 같다.

* 모델의 안정적인 성능을 위해서 single model의 KFold를 default로 하였다. 이후 hold-out set을 사용하여 valid set의 크기를 줄이고 train set의 크기를 늘려서 성능을 향상시켰다는 디스커션 댓글에서 힌트를 얻어, KFold를 약간 변형시켰다. K=5일때, train, valid set의 비율을 9:1, 9.5:0.5까지 train set을 늘려 확인해 본 결과 확실히, score가 향상되었다. 다만 이렇게 할 경우, K개의 valid set의 전체 집합이 train data 전체를 포함하지 못하며, threshold search를 할 때도 bias한 결과를 보일 수있을 거라고 생각했지만, 데이터 불균형문제와 학습데이터부족이 더 큰 문제일것이라 생각하여 최종모델도 이렇게 변형한 split을 적용하였다.  
<br/>
* 어떻게 모델을 설계할 것인가. 기존의 research들이 어떻게 접근하였는지 조사하기 위해, text classification, sentiment analysis, sentence embedding 등의 키워드로 관련 내용들을 찾아 보았다. QIQC와 거의 유사한 hate text detection과 관련한 workshop이 있어 해당 모델들을 구현해보고 실험해봤지만 딱히 이점을 얻을 수 없었다. 제너럴하게 사용되는 방식이 더 효과를 보였다. 그리고 최종적으로 baseline이 된 모델은 [A STRUCTURED SELF-ATTENTIVE SENTENCE EMBEDDING](https://arxiv.org/pdf/1703.03130.pdf)에서 사용된 모델이었다. CNN이나, capsnet 등도 테스트 해보았지만, RNN+Attention구조가 더 좋은 성능을 보였다. 위 paper의 attention 구조가 transformer의 multi-head attention이나 single attention보다 더 결과가 좋았다. 그런데 이상하게도 attention 모듈의 diversity를 위한 penalization term을 추가했을때, 추가하지 않았을 때보다 좋은 성능이 나오지 않았다. 2시간 제한시간 조건때문에 epoch의 크기가 고정되어있어서 그랬을 수도 있을 것 간다. 결과적으로 penalization term을 사용하지 않는 모델을 제출하였다. paper에서는 dropout이나 l2 regularization과 같은 regularization 기법을 사용하지 않았다고 하였지만, 나는 dropout과 batch normalization을 사용하여 좀 더 안정적인 결과를 얻을 수 있었다. batch norm이 batch에 영향을 받기 때문에, layer norm을 많이 사용하는 추세지만, batch가 클 경우 안정화된 결과를 보인다. 이 task에서는 batch를 충분히 키울 수 있었고, 실험적으로도 더 좋은 결과를 보였기 때문에 batch norm을 선택하였다. (https://arxiv.org/pdf/1803.08494.pdf: 왜 layer norm이 batch norm보다 성능이 안나왔을까에 대해 찾아보던 중 발견한 페이퍼이다.이 실험에서 figure 4를 보면 모델이 완전히 수렴하기전 batch norm이 layer norm보다 성능이 좋음을 확인할 수 있다. 어쩌면 제한시간 조건때문에 train epoch의 수를 한정시켰기때문에 이러한 결과가 나온걸 수도 있을 것이라 생각된다.) 또한 속도의 문제로 LSTM이 아닌 GRU를 사용하였다.  
<br/>
* 여러 모델들을 구현하고, 변형하면서 어쩌면 kfold변형에서 single model이 아니라 다양한 모델을 사용하면 더 좋은 성능을 낼 수 있지않을까 생각했는데(ensemble처럼), 결과적으로 가장 성능이 좋은 single model의 fold를 사용하는 것이 성능이 좋았다.  
<br/>
* extra feature의 사용. 이전의 toxic classification competition에서 상위 solution에서 extra feature(unique word rate, capital rate 등)를 사용했다는 글을 보았다. 그동안 다른 nlp task를 하거나, paper들을 공부할 때, extra feature를 사용하는 경우를 본 적이 없어서 이러한 extra feature가 과연 모델 학습에 향상을 줄지 확신을 가질 수 없었다. 실제 실험적으로도 진짜 결과가 애매했다. 그렇지만, 아주 미세한 점수차로도 순위가 크게 갈릴 수 있기 때문에, 2개의 submission중 하나는 extra feature를 사용하고 다른 하나는 사용하지 않았다.  
<br/>
* data preprocessing은 이 competition에서 매우 중요한 부분이었다. 제한시간 조건때문에 character feature를 뽑을 수 없이 오직 주어지는 pretrained word embedding에 의지해야했다. 따라서 tokenize 및 데이터를 어떻게 클렌징할것이냐가 모델 성능에 큰 영향을 미쳤다. 우선 public kernel에서는 keras의 tokenizer를 사용했지만, tokenize된 결과를 보면 그리 좋지 않다. 일반적으로 nlp에서는 nltk나 spacy의 토크나이져를 사용한다. spacy가 nltk보다 나은 결과를 주지만, nltk에 비해 느리다는 단점이 있다. 초반 모델이 제한시간에 매우 근접하였고, preprocessing에서는 시간을 최대한 줄이자는 생각으로 nltk로 선택한 후 계속 nltk tokenizer를 사용하였는데, 최종 모델이 꽤 여유롭게 돌아가는만큼 spacy로 바꿨어야했는데, 이 부분 아쉽다.ㅠㅠ (상위 모델에서는 spacy를 사용하더라.) text에 등장하는 character를 확인하면 영어뿐만 아니라 다양한 언어가 등장한다. (심지어 한글도 나온다!) 그래서 character 단위로 너무 클렌징할 경우, 오히려 좋지 않았다. unidecode로 처리하는 방법도 해봤는데 일단 public score에선 큰 차이가 없었다.
<br/>
* 데이터 불균형 문제때문에, 데이터를 샘플링하거나 weight loss나 focal loss 등을 적용해봤는데 모두 성능이 좋지 않았다.ㅠㅠ  

이 정도가 내가 생각한 kernel-only 룰 안에서 할 수 있는 범위였다. LB와 CV가 딱 맞진 않아서, 내가 조금조금씩 수정한 부분이 과연 효과가 있는지 확실할 수 없어서 어려웠다. 마지막날에 근접해서는 높은 점수의 public kernel이 공개되었기에(그러나 2-stage에서는 돌아가진 않는모델.), 그 커널의 fork가 여러번 제출되어 내 현재 스코어 위치를 가늠하기 어려워졌다.  
이후 결과와, 실제 상위모델의 방법은 다음 포스팅에서 정리하한다.
