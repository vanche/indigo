---
title: "[WIR] 내가 AI 연구를 한 첫 2년 동안 배운 교훈들 by Tom Silver"
layout: post
headerImage: false
category: blog
author: huiwon
---

몇 달전쯤 reddit에 "Lessons from My First Two Years of AI Research"이란 제목의 블로그 포스팅이 소개되었다. 공감가는 경험과 조언이 인상깊어서 요약해서 정리한다. 전문은 [원본링크](http://web.mit.edu/tslvr/www/lessons_two_years.html)에서 확인할 수 있다.  
이 포스팅은 저자가 AI분야의 커리어를 시작하는 친구에게 하는 조언이다.

## Starting out
#### 멍청한 질문도 편안하게 할 수 있는 조언자를 찾아라.
#### 다른 분야에서 리서치 영감을 얻어라.
무엇을 해야 할지 결정하는 것은 리서치의 가장 어려운 부분일 수 있다. 다음은 몇가지 가능한 전략이다.  
1.다른 분야의 연구자와 대화하라. 많은 인상깊은 연구결과는 bio/chem/physics/pure math과의 충돌에서 나온다.  
2.문제에 대한 느낌을 얻기 위해 간단한 베이스라인을 코딩해라. 종종 예상치 못한 상황(버그)를 마주할 것이다. baseline이 동작할 때, 시도해볼 다른 아이디어나, 문제에 대한 더 깊은 이해를 가질 수 있다.  
3.당신이 좋아하는 paper의 experiment section을 확장하라. 방법과 결과를 주의깊게 봐라. 먼저 가장 간단한 확장을 고려하고, paper method가 충분한지 질문하라. 그들이 미치지 못했던 것이 있을지 생각하라.
#### visualization tools과 skills에 투자하라.
시각화 스크립트를 실행시키는 것은 내가 생각했던 모델과 내 코드가 일치하는지 빠르게 확인시켜준다. 좋은 visualization은 훨씬 더 확하고 해석가능하다. 또한 예쁜 도펴나 영상을 사람들에게 보여줄 수 있으므로 동기부여도 된다. model을 최적화하고 있다면 plotting loss curve은 시작하기 좋은 지점이다. learned weight를 visual할 수도 있고, RL을 적용하고 있다면 agent가 움직이는 모습을 시각화할 수도 있을것이다.
#### 논문의 핵심 동기 파악하기
같은 컨퍼런스에 출판하고, 같은 기술용어들을 사용하는 연구자들이라도 극적으로 다른 동기를 가지고 있다. 적어도 3개의 동기 Math / Engineering / Cognitive 동기로 나눌 수 있다.

## Drinking from the research community firehose
#### 논문 찾기
[arXiv](https://arxiv.org/) : AI 페이퍼는 아카이브에서 쉽게 접할 수 있다.  
[arXiv sanity preserver](http://arxiv-sanity.com/) : Adrej Karpathy의 사이트로 arXiv의 논문들을 검색하거나 필터링하는데 도움된다.  
[Miles Bundage](https://twitter.com/Miles_Brundage) : Miles Bundage의 트위터  
[Brundage Bot](https://twitter.com/BrundageBot) : Miles Bundage의 봇  
[r/MachineLearning](https://www.reddit.com/r/MachineLearning/) : 머신러닝 레딧  
[Import AI](https://jack-clark.net/) : Jack Clark의 주간 뉴스레터  
[The Wild Week in AI](https://www.getrevue.co/profile/wildml) : Denny Britz의 주간 뉴스레터  
[NIPS](https://nips.cc/) / [ICML](https://icml.cc/) / [ICLR](https://iclr.cc/) : big conference  
[AAAI](https://aaai.org/Conferences/AAAI-18/) / [IJCAI](https://www.ijcai-18.org/) / [UAI](http://auai.org/uai2018/index.php) : reputable general-audience conferences  
[CVPR](http://cvpr2018.thecvf.com/) / [ECCV](https://eccv2018.org/) / [ICCV](http://iccv2017.thecvf.com/) : computer vision  
[ACL](https://acl2018.org/) / [EMNLP](http://emnlp2018.org/) / [NAACL](http://naacl2018.org/) : nlp  
[CoRL](http://www.robot-learning.org/) / [ICAPS](http://icaps18.icaps-conference.org/) / [ICRA](http://www.icra2017.org/) / [IROS](https://www.iros2018.org/) / [RSS](http://www.roboticsconference.org/) :robotics  
[AISTATS](https://www.aistats.org/) / [COLT](http://www.learningtheory.org/colt2018/) / [KDD](http://www.kdd.org/kdd2018/) : theoretical work  
[JAIR](https://jair.org/index.php/jair) / [JMLR](http://jmlr.org/) : journals  
[Nature](https://www.nature.com/) / [Science](http://www.sciencemag.org/) :general scientific journals  
[Google scholar](https://scholar.google.co.kr/)  

#### 논문을 읽는데 얼마나 많은 []시간을 쏟아야 하는가?
저자는 시간이 되는한 가능한 많은 페이퍼를 읽어야한다고 생각한다고 적었다. 문제에 대한 신선한 관점은 핵심키가 될수도 있으나 career researcher는 이러한 fortunate jump에 의존할 수 없다. 관련 논문들을 읽는 것은 우리의 현재 위치와 다음에 시도해볼 것을 파악할 효율적인 방법이 될것이다. 하나 더 중요한 것이 있다. 논문을 읽는것만큼 그것을 소화시키는 것이 중요하다. 소수의 논문을 주의깊게 읽는 것이 좋다.

#### Conversation >> videos > papers > conference talks
논문은 낯선 연구 분야를 이해하는데 가장 접근하기 쉬운 방법이다. 하지만 문제를 이해하는 사람과의 대화가 훨씬 효과적이다. conference talks의 경우 교육적인 기회보다는 형식적인 경우가 많다.

#### Beware the hype
과장광고 조심하라.

## Running the research marathon
#### 측정할 수 있는 진행과정을 만들어라.
무엇을 할지에 대한 매우 작은 아이디어라도 있다면, 가능한 디테일하게 적어라. 배제하는 아이디어라면 그 이유를 적어라. 진행과정은 논문 읽기나 대화의 형태일 수도도 있다. 매일 실제하는 증거를 남기면, 그 아이디어들이 사용되지 않더라고 사기가 진작되고, 같은 아이디어에 대한 사이클을 도느라 시간을 낭비하지 않을 것이다.

#### 막다른 길을 인식하고 되돌아가는 것을 배우기

#### 적어라!

#### 정신/신체건강은 연구의 전제조건이다.
