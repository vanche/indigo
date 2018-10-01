---
title: "How to Win a Data Science Competition 강좌 week1 요약정리"
layout: post
headerImage: false
category: blog
author: huiwon
---
캐글 상위권에 올라가기 위해서 좀 더 체계적인 공부가 필요한것 같아, 많은 이들이 추천하는 강의인 coursera의 How to Win a Data Science Competition:Learn from Top Kagglers 강의를 듣기 시작했다. 이 강의는 실제 Kaggle에서 상위랭커들이 직접 자신들의 노하우를 포함하고 있다. 캐글러들 대부분이 상위권 캐글러의 포스팅이나 커널, 인터뷰들을 참고하여 성장하는만큼 이 강의가 내게 도움이 되길 바란다. 이 포스팅인 해당 강의의 내용을 요약정리한 것이다.  

### Introduction
이 강좌에서는 competition solving process를 단계별로 배운다. 다루는 내용은 다음과 같다.
+ EDA
+ feature generation and preprocessing
+ model validation techniques
+ data leakages
+ metric optimization
+ model ensembling
+ hyperparameter tuning  

이 강좌는 advanced students를 기준으로 설계되었다. 따라서 이미 머신러닝에 대해서 친숙하다고 가정한다. competition에서 이기기 위해서는 매우 열심히, 또한 효율적으로 작업해야 한다. 그리고 **Everyone who works hard will learn a lot.**

### Course Overview
+ Week 1  
-Introduction Competition : Competition에 대해 설명하고 real-life problem과 어떻게 다른지 비교한다.  
-Feature preoprocessing & extraction  
+ Week 2  
-Expolaratory Data Analysis  
-validataion strategies  
-Data leaks  
+ Week 3  
-Metrics  
-Mean-encodings  
+ Week 4  
-Advanced features : statics, distance-based features, metric factorizations, feature interactions, t-SNE 등을 포함한다.  
-Hyperparameter optimization  
-Ensembles  
+ Week 5
-Final project
-winning solutions

### Competition mechanics
+ Data : csv파일, 이미지 셋 등. competition 규칙에 위배되지 않는다면 외부 데이터를 이용할 수 있다.  
+ Model : 모델은 가장 좋은 prediction을 생산할수 있어야하고 reproducible해야한다.
+ Submission : 보통 prediction output을 제출하여 자신의 모델의 성능을 평가한다.(때때로 코드를 제출하기도 함.)  
+ Evaluation : 모델의 성능은 evaluation function에 의해 평가된다. 다음과 같은 metric이 있다.
  + Accuracy
  + Logistic loss
  + AUC
  + RMSE
  + MAE
+ Leaderboard : 리더보드에서 현재 순위를 확인할 수 있다. (best score를 기준으로 표시된다.) 리더보드는 단순 순위 뿐만이 아니라 데이터셋의 정보 또한 드러내게 만든다. ground truth를 얻기 위해 많은 submission을 내는 경우를 방지하기 위해 test set은 public set과 private set으로 나누어진다. private set은 competition이 종료된 후 공개된다.
#### Platforms
Kaggle 외의 다른 platform으로 DrivenData, CrowdAnalityx, CodaLab, DataScienceChallenge.net, Datascience.net, Singel-competition sites(like KDD, VizDooM)이 있다.
#### 왜 참가하는가?
+ 배우고 네트워킹할 기회를 얻기위해
+ non-trivial tasks와 state-of-the-art approaches
+ data science community에서 유명해질 기회
+ 상금 ㅎㅎ
#### Real world application  vs Competition
+ real world ml pipeline : 비지니스 문제를 이해하고 문제를 형식화한다. 데이터를 수집하고 전처리한다. 모델링하고, 실제 상황에서 모델을 평가한 후 배포한다.
+ competition은 위 파이프라인중 데이터 전처리와 모델링만을 포함한다. 그러나 인사이틀 얻거나 new feature를 뽑기 위해서는 비지니스 문제를 이해할 필요가 있다. 또한 몇몇 대회는 external data 사용을 허락하기에 데이터 수집단계가 필요하기도 하다.
#### 알고리즘만이 중요한것이 아니다.
+ 데이터를 이해하는 것이 중요하다.
#### 제한하지 말기
+ Heuristics이나 manual data analysis도 괜찮다.
+ complex solution이나 advanced feature engineering, doing huge calculation을 두려워하지 말라.


### Recap of ML algorithm
+ Linear model : 선형 모델의 메인 아이디어는 object가 있는 공간을 두 파트로 나누는 것이다. 선형 모델로 logistic regression과 SVM이 있다. 선형모델은 sparse한 high dimentional data에 좋다. scikit-learn에 대부분의 linear model 라이브러리가 구현되어 있다. vowpal wabbit는 large data sets을 다루기 적합한 라이브러리이다.  
+ Tree-based - Decision Tree, Random Forest, GBDT : decision tree는 divide-and-conquer방식을 이용해 sub-split space를 다시 subspace로 쪼갠다. tree-based model를 많은 splits을 필요로 해서 linear dependencies를 확인하기 어렵다. 관련 라이브러리로 scikit-learn, xgboost , lightGBM이 있다.
+ k-NN-based method : kNN모델은 직관적으로 closer object는 같은 label을 갖고 있을 확률이 높다는 사실을 이용한다. 관련 라이브러리로 scikit-learn이 있다.
+ Neural Networks : image, sounds, text 등에 적합하다.  
the most powerful method are gradient boosted decision trees and neural networks. but don't underestimate others.  

**모든 테스크에서 좋은 성능을 낼 수 있는 method는 없다. 각각의 method가 어떠한 데이터나 테스크 특성에 대해 가정을 가지고 시작되기 떄문에 그 가정이 틀리다면 좋은 성능을 낼 수 없기 때문이다. No silver buller**  
+ 추가 resource : [random forest](https://www.datasciencecentral.com/profiles/blogs/random-forests-explained-intuitively), [gradient boosing](http://arogozhnikov.github.io/2016/06/24/gradient_boosting_explained.html), [knn](https://www.analyticsvidhya.com/blog/2018/03/introduction-k-neighbours-algorithm-clustering/)
그외 : sklearn documentation의 knn, linear model, decision trees 링크 , tools 링크 제공. 이건 필요할 때마다 보기로 하자.

#### GBDT
GBDT는 $m+1$번째 트리를 기존 모델을 향상시키도록 다음의 식 $F_{m+1}(x)=F_m(x)+h(x)=y$ 을 만족하도록 한다. 즉 $h(x)=y-F_m(x)$이고, $h$가  residual $y-F_m(x)$에 fit하게 된다. approximation $\hat{F}(x)$는 $h_i(x)$의 weighted sum이고, 즉 다음과 같다.$$\hat{F}(x)=\sum_{i=1}^M \gamma_{i}h_i + const$$
이 때 $\gamma$ 가 learning rate 와 같다. 만약 learning rate가 클 경우 GBM에서 first tree를 제외한 결과는 last tree를 제외한 경우보다 성능을 더 떨어뜨린다.

### Software / Hardware requirements
+ Ram : if you can kep data in memory - everything will be much easier. 16GB+이상 권장. 강사의 경우 64GB정도가 적당하다고 생각하나 일부 프로그래머는 128GB이상을 선호.
+ Cores : more cores you have - more experiments you can do.
+ Storage : ssd 추천. 특히나 image데이터를 다룰 때는 더더욱 필요하다.
+ software : 언어로는 당연하게 파이썬 추천. 프로토타이핑이 쉽고, 방대한 라이브러리를 보유하므로.

### Assignment
1주차 assignment에서는 "Predict Future Sales"이라는 kaggle playground prediction competition의 데이터를 이용해 문제의 답을 구하는 것이다. pandas를 이용해서 데이터를 가공할 수 있어야 문제를 풀 수 있다. EDA를 통해 data특성을 하나하나 보여주는 것이 아니라 문제를 통해 데이터를 가공할 방법을 접근하도록 하여 데이터를 파악하게 해준다.

### Feature preprocessing and generation with respect to models
#### Numeric features
+ Preprocessing : scaling
tree-based model의 경우 feature scale에 영향을 받지 않는다. 반면 knn, linear models, neural networks와 같은 non-tree-based model의 경우는 다르다. 예를 들어 2차원 평면 위의 좌표를 knn을 통해 classification하는 경우, x좌표값에 특정값을 곱하면 결과가 달라질 수 있다. gradient descent method의 경우에도 적절한 스케일링 필요하다.  
feature scaling에 따라 모델의 결과가 다를수 있다.
  1. To [0, 1]
  $$ X=(X-X.min())/(X.max()-X.min()) $$
  이러한 feature들을 다루는 가장 쉬운 방법은 feature를 MinMaxScaler를 이용해 same scale로 리스케일링하는것이다.(즉 value범위가 0~1사이에 위치하게 되면서 distribution은 변하지 않는다.)
  2. To mean=0, std=1
  $$ X = (X-X.mean())/X.std() $$
  StandardScaler를 사용할 수도 있다.
모든 feature를 하나의 방식으로 스케일링해야 feature는 비슷하게 모델에 영향을 끼치게 된다.  
+ Preprocessing : outlier
linear모델에서 outlier 값을 처리해주기 위해서 lower, upper bound를 넘어가는 값을 clipping해줘야 한다. 예를 들면 1%, 99%를 기준으로 처리할 수 있는데, 이러한 방식의 clipping은 financial data에 잘 사용되고 winsorization이라고 부른다.
+ Preprocessing : rank
rank transformation은 outlier가 있으면 MinMaxScaler보다 나을 수 있다. outlier를 수동적으로 조작하기 힘들다면 rank transformation으로 linear model, knn , neural network와 같은 모델에 적용하여 이익을 볼 수 있다. scipy.stats.rankdata로 rankdata를 적용할 수 있고, rankdata를 train data뿐만 아니라 test data 또한 함께 이용해야함을 주의해야 한다.
+ Preprocessing
다음과 같은 transform으로 너무 큰 값을 가지고 있는 데이터를 처리할 수 있다.
  1. Log transform : np.log(1+x)
  2. Raising to the power < 1:  np.sqrt(x+2/3)
+ Feature generation
선지식이나 EDA를 통해 얻은 지식이나 직관력을 바탕으로 new feature를 만들 수 있다. 예를 들어 부동산의 크기와 가격을 바탕으로 평당가격이라는 새로운 feature를 만들 수 있다. 이런식으로 생성한 feature는 linear model 뿐만 아니라 다른 모델에도 효과적이다. 왜냐면 gradient boostring decision tree도 multiplication이나 division과 같은 approximation에 어려움을 겪기 때문이다. feature generatoin의 또 다른 예는 price의 fractional_part를 뽑는 것이다.
