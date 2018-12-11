---
title: "How to Win a Data Science Competition 강좌 week4 정리"
layout: post
headerImage: false
category: blog
author: huiwon
---

### Data leakages
Data leakages는 간단히 leaks라고도 불리는데, 데이터의 예상치못한 정보로 인해 비현실적으로 좋은 예측을 만드는 경우를 말한다. 직간접적으로 ground truth가 test data에 추가된 경우가 그렇다. 대체로 실수나 사고에 의해 발생된다.
#### Leaks in time seires
* 실생활에선, 미래의 정보를 알 수 없지만, competition의 경우는 그렇지 않을 수 있다. train/public,private split이 제대로 이루어졌는지 확인해야 한다.  
* split이 제대로 이루어졌어도, feature가 미래에 대한 정보를 가지고 있을 수 있다. 예를 들면 ctr task에서 user history라던가, 주식시장예측task에서 fundamental indicator들이 그러하다.
#### Unexpected information
드물게 나타는 leaks type으로 다음과 같은 경우도 있다. 이 경우 좀 더 확인하기 힘들다.
*  Meta data : meta information은 target varible과 연결되어 있기도 한다. 그래서 이미지를 리사이즈 하거나 creation date를 변경하는 노력을 가한다.
* Information in IDs : ID는 단순히 해쉬값일 수 있고, target variable과 연결된 정보의 자취를 포함할 수도 있다. Caterpillar competition의 경우가 그러했다.
* Row order : data는 target variable에 의해 shuffle되어 있을 수 있다. 때때로 row number나 relative number를 추가하는것으로 결과가 좋아지기도 한다. Telstra Network Disruptions competition의 경우가 그러했다. row가 중복되어 주어진 경우도 있었다.

### Leaderboard probing and examples of rare data leaks
data leak과 관련한 competition-specific technique를 알아본다.
#### Leaderboard probing
Leaderboard probing에는 두가지 유형이 있다. 하나는 Leaderboard의 public part에서 모든 ground truth를 추츨하는 것이다. (aleck trott의 post를 확인할것) 여기선 다른 유형에 대해 초점을 맞춘다. private data에 대한 정보를 얻기위해 submission을 이용할 수 있다. 모든 row에 같은 target값을 가진 data chunk를 생각해보자. 예를 들면 같은 ID를 가진 row는 같은 target을 가진 경우가 있다. public data와 private data가 중복될 경우, submission결과로 private data의 target값을 알 수 있을 것이다. 이렇게 일관성을 가진 categorical value가 있을 경우 이러한 경우를 조우할 수 있다.(ex. redgat / west nile competition) consistent category는 단순히 같은 category일 때 같은 target값을 같는 경우뿐만 아니라 넓은 의미로 생각할 수 있다. 예를 들어 target label은 public과 private data에서 같은 distribution을 가질 수 있다. Quora Question Pairs competition의 경우가 그러했다. 이 대회는 log loss metric을 사용하는 이진분류 문제였다. test set을 consistent category로 다루어 큰 이득을 볼 수 있었던 대회였다.
* Adapting global mean via LB probing  
logarithmic loss for submission with constant prediction C.  
N : real number of rows  
N1 : the number of rows with target one  
L : Leaderboard score given by that constant prediction  
$$-L*N = \sum_{i=1}^N(y_i\ln C + (1-y_i)\ln (1-C))$$ $$-L*N = N_1\ln C + (N-N_1)\ln (1-C)$$ $$\frac{N_1}{N}=\frac{-L-\ln (1-C)}{\ln C - \ln (1-C)}$$
training data points를 재조정하여 test set의 target variable의 distribution과 같게 만들어 lb score에서 boost를 이루었다.  
#### Peculiar examples
* Truly Native
html 파일이 스폰서받은 것인지 아닌지 예측하는 문제. archive dates에서 data leak이 있었다. 다른 기간동안 sponsored, non-sponsored파일이 모아졌고, date에 관한 정보가 완전히 제거되지 못하였다.
#### Expedia
고객이 검색한 히스토리와 검색결과에 대한 반응에 해당하는 정보가 주어졌을때, 누가 예약을 진행할것인지에 대해 예측하는 문제였다. user와 검색하는 호텔사이의 거리 feature에서 data leak 이 있었다. 두 좌표를 리버스 엔지니어링하여, ground truth을 간단히 mapping할 수 있었다. 이 competition에 관한 스페셜 영상을 참고할 것.
#### Flavours of physics
organizer는 ml로 관측되지 못한것을 예측하길 원했기에 시그널이 인공적으로 생성되었다. 그러나 시뮬레이션이 완벽하지 못했고 리버스 엔지니어링이 가능했다. 이에 시뮬레이션 결함을 이용하는 모델에 패널티를 주려고 했지만 헛된 시도였다. 누군가 이 결함을 이용해 perfect score를 만들어버렸다.ㄷㄷ
#### Pairwise tasks
Quora question pairs competition은 아이템 페어가 중복되었는지 맞추는 대회다. 참가자에게 모든 pair에 대해 예측하라고 하지 않는다. 즉, nonrandom subsampling 이 있고, 이 subsampling이 data leak의 원인이 된다. organizer가 pair를 구별하여 샘플링하기 어렵기 때문에, 아이템 빈도에서 불균형이 발생하고, 좀 더 많이 나온 아이템일수록 중복된 아이템일 가능성이 높다. 또한 각각의 row, column을 item의 번호로 하는 N by N Matrix를 만들어 vector간 유사성을 계산할 수 있다. 즉 같은 이웃을 가진 페어는 중복일 가능성이 높다.
