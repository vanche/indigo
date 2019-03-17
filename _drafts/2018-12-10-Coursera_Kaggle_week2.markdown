---
title: "How to Win a Data Science Competition 강좌 week4 정리"
layout: post
headerImage: false
category: blog
author: huiwon
---
### Exploratory data analysis
2주차에서는 대회 풀이 과정에 필수적인 요소인 EDA에 다룬다. 또한 competition setup에 따라 validation을 어떻게 디자인하는지 살펴보고, data competition에서 매우 독특한 주제인 data leakage에 대한 내용을 커버한다.
#### Overview
EDA에서 다룰 내용들은 다음과 같다.
1. EDA가 무엇이고 왜 필요한지
2. things to explore
3. exploration and visualization tools
4. dataset cleaning
5. Kaggle competition EDA  
#### Exploratory Data Analysis (EDA)
EDA란 데이터를 분석하고 이해하는 과정이다. competition에선 powerful feature를 만들어 더 정확한 모델을 설계할 수 있어야 한다. 데이터를 분석하면서, 데이터에 대한 직관력을 얻고 이를 통해서 가능한 새로운 feature에 대한 가정을 만들 수 있다. raw competition에 경우 EDA가 특별히 도움되진 않을 수 있다. 이 경우엔 모델링과 서버를 만드는데 시간을 더 들이는게 낫다.
#### Visualizations
데이터를 시각화하여, 패턴을 읽고 이 패턴을 어떻게 이용할지 생각해 볼 수 있다.
#### Motivating examples
data를 잘 이해했다면 어떤 모델링도 필요없었던 대회도 있었다. 이 대회의 목적은 사람들이 company가 제공한 promo를 어떻게 이용할지 예측하는 대회였다. data의 row는 각 사람들에게 제공한 promo에 대한 정보와 person에 대한 정보(성별, 나이, 결혼유무 등)와 target(0 or 1)값을 가지고 있었다. column feature 중 #promos sent(현재까지 보내진 promo수) feature와 #promos used(이전까지 사용된 promos)가 있었다.  
| id | ... | # promos sent | # promos used |                diff                | used this promo? |
|:--:|:---:|:-------------:|:-------------:|:----------------------------------:|:----------------:|
| 13 | ... |       0       |       0       | <span style="color:green">1</span> |         1        |
| 13 | ... |       1       |       1       | <span style="color:green">0</span> |         0        |
| 13 | ... |       2       |       1       |  <span style="color:red">1</span>  |         0        |
| 13 | ... |       4       |       2       | <span style="color:green">1</span> |         1        |
| 13 | ... |       5       |       3       | <span style="color:green">0</span> |         0        |
| 13 | ... |       6       |       3       | <span style="color:red">NaN</span> |         0        |  
위 표처럼 id에 대하여 정렬할 경우, diff feature(인접한 row간 promos used 차이)를 만들 수 있었고, 이 feature만을 사용하여 80%의 정답률을 가질 수 있었다. (missing data나 NaN데이터를 어떻게 처리하냐의 문제가 남아있긴했지만, 어떤 classifier도 사용않고도 높은 acc를 얻을 수 있었다.) 이러한 featrue는 EDA없이는 생성될 수 없다. 한편 위와 같은 문제는 organizer가 데이터를 준비하는 과정에서 일어나는 실수에 의해 발생하는데, 이를 leak이라 하며 competition 상에서 이용이 허락된다.
### Building Intuition about the data
#### Get domian knoledge
Kaggle에서는 다양한 도메인의 문제를 다룬다. 그 도메인의 깊은 지식이 꼭 필요하진 않다. 다만, 우리의 목적과 우리가 가진 데이터를 이해하고, 베이스라인을 세우기 위해 사람들이 주로 이러한 문제를 어떻게 접근하는지 알 필요가 있다.
#### Check if the data is intuitive
age 데이터에서 200살이 넘은 데이터가 있는 경우, Google Ads Data에서 Impression(노출된 횟수) < Clik수 인 경우 데이터에 버그가 있는 경우일 것이다. 후자의 예는 전자의 예와는 다르게 random하게 발생된 버그가 아닐 수 있다. data exporting script나 다른 알고리즘에 의하여 발생한 것일 수 있다. 이러한 실수를 이용하여 is_incorrect라는 새 feaure를 만들어 더 좋은 score를 얻을 수도 있다.  
#### Understand how the data was generated
데이터가 어떻게 생성되는지 이해하는 것도 중요하다. 데이터는 랜덤하게 샘플링될 수도 있고, 데이터 클래스의 균형을 맞추기 위해서 oversampling되었을 수도 있다. 데이터가 어떻게 생성되었는지 파악해야 적절한 validataion scheme을 세울 수 있다. 만약 test set과 train set이 다를 경우(distribution이?), validation set이 test set의 respresentation과 맞지 않을 수 있다. 즉 CV에서 향상되었으나 LB에서는 그렇지 않을 경우, 혹은 CV보다 이상하게 LB score가 높을 경우가 발생할 수 있다.
### Exploring anonymized data
데이터는 익명화되어 주어지기도 한다. 예를 들어 organizer가 text 데이터를 공개하길 꺼려할 경우 text data는 hash처리되어 주어진다. (이렇게 변환되도 bag of word를 base로 하는 모델은 달라지지 않는다.) 정형데이터의 테이블의 column(과 이름)이 익명화될 수도 있다. 이 경우 data를 decode하거나 de-anonymize를 시도해 볼 수도 있다. 이것이 불가능한 경우에도 feature의 타입을 추측하여 numeric, categorical 타입등으로 분류할 수 있다. 그리고, feature들 간의 관계를 조사하거나 그룹핑해볼 수 있다.


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

### Visualizations
데이터에서 흥미로운 정보를 찾는 정해진 레시피는 없다. 시간을 들여 데이터를 보고, 찍어보고, 실험해 봐야한다. 따라서 EDA는 art다. 이 때 사용하는 다양한 시각화 툴이 있다. 첫번째로, histogram으로 표현해 볼 수 있다. 히스토그램은 feature를 bin으로 쪼개므로 모든 feature가 한곳에 모여있게 보이도록 잘못 표현되기도 한다. 이 때엔 feature에 log를 취하여 데이터를 다시 확인할 수 있다. 즉 한번의 plot만으로 결론을 내려서는 안된다. 가정을 있다면, 여러번 다르게 plot하여 이를 증명해야한다. mean값 처럼 특별한 값에 peak가 있는 경우는 organizer가 missing value가 있는 데이터에 평균값을 채워넣은 경우로 이해할 수 있다. 우리는 이 정보를 이용하여, 다시 그 값을 missing value로 채워넣을 수 있다. missing value를 처리하는 boosting알고리즘을 쓸 경우 이들을 볼 수 있을지 모른다. 또는 mean값이 아닌 다른값(-999)으로 채워넣거나 그 값이 missing value였다는걸 가리키는 새로운 new feature를 생성할 수 있다. 두번째 시각화 방법은, x축을 row index로 y축을 feature value로 표현하는 것이다. 만약 수평선을 확인할 수 있다면 feature의 특정값이 많이 반복됨을 의미한다. 수직선이 보인다면 데이터가 제대로 shuffle되지 않은 것이다.

### Dataset cleaning and other things to check

### Reading Materials
* visualization tools
[seaborn](https://seaborn.pydata.org/)
[Plotly](https://plot.ly/python/)
[Bokeh](https://github.com/bokeh/bokeh)
[ggplot](http://ggplot.yhathq.com/)
[Graph visualization with NetworkX](https://networkx.github.io/)
* others
[Biclustering algorithms for sorting corrplots](https://scikit-learn.org/stable/auto_examples/bicluster/plot_spectral_biclustering.html)
[]()
[]()
[]()
[]()
