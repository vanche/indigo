---
title: "NLP Pretrained Model : BERT (2)"
layout: post
headerImage: false
category: blog
author: huiwon
---
이전 포스팅에서는 BERT 의 전체적인 구조에 대해서 정리하였다. 이번 포스팅에서는 BERT가 실험한 NLP task에서 fine-tunning을 적용한 방법과 결과를, 그리고 ablation studies까지 정리하여 BERT paper에 대한 정리를 마무리한다.
## Experiments
BERT는 GLUE와 squad 를 포함하여 총 11가지 NLP task에 대해서 실험을 하고 결과를 보고하였다.  
### GLUE Datasets
GLUE(Ganeral Language Understanding Evaluation) benchmark는 말 그대로 다양한 nlu task의 집합체다. GLUE의 대부분의 dataset은 이미 존재하던것이었지만, train, dev, test splits으로 나눠지고, 평가에 일관성이 없음과 test set overfitting 이슈를 완화시키기 위해 evaluation server 를 셋업되었다.  
GLUE는 다음의 데이터셋을 포함한다.
* MNLI(Multi-Genre Natural Language Inference): entailment classification task  
* QQP(Quora Question Pairs): Quora에 올라온 질문 페어가 의미적으로 동일한지 확인하는 테스크
* QNLI(Question Natural Language Inference): SQuAD의 이진분류 버전. paragraph가 answer를 포함하는지 안하는지 확인하는 문제.
* SST-2(Stanford Sentiment Treebank): 단문장 이진분류문제. 영화리뷰에서 추출된 문장에 감정이 표기되어있음.
* CoLA(Corpus of Linguistic Acceptability): 영어문장이 언어학적으로 acceptable한지 확인하는 이진분류문제
* STS-B(Seemantic Textual Similarity Benchmark): 문장쌍이 얼마나 유사한지 확인하는 문제.
* MRPC(Microsoft Research Paraphrate Corpus): 문장쌍의 유사성 확인하는 문제.
* RTE(Recognizing Textual Entailment): MNLI와 유사하나 데이터가 적음.
* WNLI(Winograd NLI): 자연어 추론 데이터셋이나 현재 채점에 이슈가 있어서 BERT 실험에서는 제외됨.  

### GLUE Results
GLUE에 fine-tunning하는 방식은 아래의 그림과 같다.

첫번째 input 토큰([CLS])에 해당하는 final hidden vector $C$를 aggregate representation으로 사용한다. 이 $C$에 classification loss를 계산한다. (즉, $log(softmax(CW^T))$)  
BERT_LARGE모델의 경우 small data set에 안정적이지 않아, 여러번 random restart하여 dev set에 가장 성능이 좋은 모델을 선택하였다. (random restart에서 pre-trained model의 같은 checkpoint를 사용하지만, 다른 데이터셋 셔플링과 다른classifier layer 초깃값을 사용한다.)
