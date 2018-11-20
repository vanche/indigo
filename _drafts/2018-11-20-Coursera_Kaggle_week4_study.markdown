---
title: "How to Win a Data Science Competition 강좌 week4 요약정리"
layout: post
headerImage: false
category: blog
author: huiwon
---

### Ensembling

#### Stacking
스태킹은 competition에서 매우 인기있는 부스팅 방법이다. 스태킹이랑 hold-out data sets((train, valid), test set으로 나누는 방식)으로 several prediction 을 만들고, 새로운 data sets을 만들기 위해 이 prediction을 stacking or collecting한다. 이 새로운 데이터셋으로 new model이 fit하도록 훈련시킨다. 이전에 설명했던 방식들과는 다르게 새로운 모델을 훈련시키기 위해 오직 prediction만을 사용한다. meta model이 예측기들이 historical한 정보 알고있다면, input data없이도 prediction을 종합할 최선의 방법을 찾을 것이다.
+ Methodology  
  1992년에 Wolpert가 스태킹을 처음 제안했다.
  1. data set을 train set과 validation set으로 disjoint하게 분리한다.
  2. base learner들을 훈련시킨다.
  3. base learner로부터 validation dataset 에 대하여 prediction을 얻는다.
  4. 3에서 얻은 prediction을 higher level learner(meta model)의 input으로 사용한다.
+ Still confused about stacking?  
  data set을 A(train set), B(valid set), C(test set)으로 나누었다고 가정한다. algorithm0,1,2를 각각 A에 대하여 훈련시켜서 B, C에 대한 예측인 B1, C1을 각각 얻는다. algorithm3를 B1에 대하여 훈련시켜, C1에 대한 예측을 얻는다.
+ stacking을 사용할 때 주의해야할 점  
  + time sensitive data를 다룰 때 시간을 고려하게 데이터를 배치한다.
  + 모델의 다양성은 성능만큼이나 중요하다.
  + 다양성은 다음에서 온다: 다른 알고리즘 /다른 input features
+ stacking을 쌓는데는 제한이 없지만, 어느순간 더이상 의미있는 증가를 보이지 않는 순간이 올 것이다.
+ meta model은 deep할 필요없다.

#### StackNet
multi model을 multi level의 neural net 구조로 결합하는데 스태킹을 활용하는 확장성 있는 meta modeling 방법론이다.
+ Why would this be of any use?
  multiple level의 deep stacking은 competition에 도움이 된다.
+ StackNet은 neural network같이 표현될수 있다. Neural Net에서 every node는 simple linear model(with some non linear transformation)이다. 대신에 StackNet은 linear model이 아닌 어떤 모델도 사용할 수 있다.
+ How to train
  + 모든 모델이 미분가능한 것은 아니기 때문에 back propagation을 사용할 수 없다.
  + 따라서 가각의 모델/노드와 target을 연결하는 stacking을 사용한다.
  + 레이어를 쌓아 트레이닝하기 위해선, training/valid set 나누었던 데이터셋을 다시 사용하기 위해서 valid set을 다시 mini train/ mini valid로 나누어야 하는데, 데이터가 많지않은 일반적인 경우 이것은 문제가 될 수 있다. 여기서 K-Fold 패러다임을 이용하는 이유이다. K-Fold를 이용해 whole prediction을 얻으면, whole training데이터를 one last model을 트레이닝하기 위해 사용하고 test data에 대한 prediction을 얻을 수 있다.
  + No epochs - 대신에 different connection이 있다.
  + 꼭 이전 level과 연결될 필요가 없다.
