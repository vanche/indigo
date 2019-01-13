---
title: "NLP Pretrained Model : Bert, GPT, ELMo, ULMFiT, Semi-supervised sequence learning.(1)"
layout: post
headerImage: false
category: blog
author: huiwon
---
작년 11월, 구글에서 발표한 NLP Model인 BERT로 인해 AI/NLP 커뮤니티들이 시끌벅적했었다. 기존 NLP문제들은 각각의 task를 풀기 위한 구조를 설계하는 방식이었는데, Bert는 하나의 pretrained model에 기반하여 fine-tunning만으로 여러 task에서 SOTA를 기록함으로써 NLP의 판도를 완전 바꾸어 놓았다. 누군가는 지금까지 나왔던 nlp model 중 best라고 칭송할만큼, BERT는 연신 화제였다. 이 포스팅 시리즈에서는 BERT 를 우선 정리하고, BERT 이전의 language pretrained model이었던 GPT와 ELMo, ULMFiT 등에 대해 정리한다.

# BERT : Pre-tarining of Deep Bidirectional Transformers for Language Understanding
## Abstract
BERT는 **B**idirectional **E**ncoder **R**epresentations form **T**ransformer의 약자이다. 그 이름 그대로 BERT는 transformer 의 인코더를 multi layer로 쌓은 구조를 취한다. (참고로 bert는 sesame street라는 프로그램의 캐릭터이기도 하다. 이전에 feature extranction방식인 nlp pretrained model인 ELMo(역시 같은 프로그램의 캐릭터이름)가 등장한 이후, sesame street의 캐릭터 이름들이 pretrained model 이름에 붙고 있다. 얼마전 MS에서 big bird라는 이름으로 리더보드에 등장하기도 했다.) BERT는 모든 레이어에서 좌우 문맥을 모두 취하여, deep Bidirectional representation을 pre-train하도록 디자인 되었다. 이 pre-trained representation에 하나의 output alyer를 추가하여 fine-tunning하는 간단한 방식으로 QA, language inference등 다양한 task에서 state-of-the-art를 찍었다. (squad 1.0에선 human performance를 넘기도 하였다.) 또한 구글에서 github에 bert open source를 공개하기도 했다.(그러나 실제 페이퍼와 완전 동일한 모델은 아니다. 실제 구글에서는 텐서플로우로 작업하지 않았음을 언급하였고,(c++을 사용했다고 한다. 파라미터값들도 동일한지 확인되지않았다.)

## 1. Introduction
language model pre-training은 많은 nlp task에서 성능향상을 보여왔다. (semi supervied sequence learning, ELMo, ULMFiT, GPT 등) pre-trained language representation을 downstream task에 적용시키는 방법으로 크게 두가지가 있는데 하나는 feature-based 방식이고 다른 하나는 fine-tunning방식이다. ELMo가 전자의 방식을 사용하며, 기존 task-specific한 모델구조에 pre-trained representaition을 추가적인 feature로 사용한다. 후자인 fine-tunning방식은 GPT에서 사용되었는데, 최소한의 task-specific한 파라미터를 pretrained parameter에 더해 fine-tunning하는 방식이다. 논문저자는 기존 LM(language model)의 단방향 구조를 사용한것이 가장 주요한 한계포인트였음을 지적한다. 특히 openai의 GPT가 left-to-right구조로 모든 토큰이 이전 토큰만을 self-attention하기 때문에 양방향의 문맥을 종합하여야하는 SQuAD와 같은 QA task 에 치명적이었을것이다. 이에 BERT는 입력의 토큰을 랜덤하게 masking하여 이 masking된 단어를 예측하는 masked LM을 pre-trained model 의 objective로 사용할 것을 제안하였다.(1953 taylor의 Cloze task에 영감을 받았다.) 이로써 deep bidirectional transformer를 사용할 수 있게 되었다.  
이 논문의 기여한 바를 정리하면 다음과 같다.
* language representation을 위한 pre-training 에서 양방향의 중요성을 보인다. Bert는 단방향이 GPT와 다르게 Masked LM을 사용하여 양방향으로 학습시켰다. ELMo가 left-to-right, right-to-left를 독립적으로 학습시킨 concatenation 구조인 것과도 비교된다.  
* pre-trained representation은 task-specific한 구조의 필요성을 제거할 수 있음을 보였다.
* 11개의 NLP task에서 sota를 기록하였다.

## 2. Related work
### Feature-based Approaches
nlp모델에서 pretrained word embedding을 사용하는 것은 필수불가결한 요소로 여겨져왔다. ELMo는 language model에서 context-sensitive feature를 추출하여 사용할 수 있음을 보였다. SQuAD를 비롯하여 sentiment analysis등 다양한 task에서 sota를 보였었다. (한떄 이런 task 리더보드에서 elmo를 덧붙인 모델들이 상위권을 이루었었다.)  
### Fine-tuning Approaches
최근 트렌드는 LM objective의 모델 구조를 pre-train한 후, 같은 모델을 supervied ownstream task에 맞게 fine-tunning하는 방식이다. (semi supervied sequence learning, GPT, ULMFiT 등) 이러한 모델의 장점은 scratch부터 학습되는 파라미터가 매우 적다는 것이다. GPT는 GLUE benchmark의 여러 sentence-level task에서 sota를 기록했었다.
### Transfer Learning from Supervised Data
NLI, NMT, Vision task에서 supervised task에서 pretrain된 모델의 transfer를 하는 방식이 효과가 있음을 보여왔다.

## 3. BERT
### Model Architecture
BERT는 multi-layer Bidirectional Transformer encoder 구조로 되어있다. 최근 transformer가 아주 흔하게 사용되고 bert도 동일한 구조를 사용하기 때문에 논문에서 transformer에 대한 자세한 언급은 생략한다. transformer의 구조에 대해 좀더 자세히 알고싶다면, transformer paper(attention is all you need)나 Annotated transformer(transformer에 대해 코드와 함께 좋은 가이드를 준다.)를 참고할 것. BERT는 parameter를 달리하여 BERT_base와 BERT_large로 구분된다. (BERT_base 모델은 GPT와 동일한 모델사이즈를 가져, 둘을 비교하기 쉽게 한다.) BERT의 transformer와 GPT의 transformer가 다른 점은 BERT쪽은 bidirecitional self-attention 을 취하지만, GPT의 것은 왼쪽 방향만으로 self-attention을 취한다는데 있다. 이 점 때문에 BERT에서는 자신들의 것을 transformer encoder라고 부르고 GPT의 것을 transformer decoder라고 부른다.
### Input representaition


*[Open Sourcing BERT](https://ai.googleblog.com/2018/11/open-sourcing-bert-state-of-art-pre.html)
