---
title: "[Kaggle] Home Credit Default Risk"
layout: post
headerImage: false
category: blog
author: huiwon
---
### Description
신용 거래 내역이 없는 고객들의 통신/거래 내역 등의 대체 정보로 해당 고객이 상황 능력이 있는지 예측하려 한다.

### Data
* application_{train|test}.csv : main table로 각 행은 하나의 대출이력을 보여주며 SK_ID_CURR feature로 식별된다. target이 1일 경우 client가 상환 힘든 케이스이고 0일 경우 다른 케이스이다.
* bureau.csv : Credit Bureau에 보고된 client의 이전 신용정보를 포함한다.
* bureau_balance.csv : Credit Bureau에 보고된 이전 신용 정보의 월간 기록을 포함한다.
* POS_CASH_balance.csv : POS(point of sale) 월간 데이터를 포함한다.
* credit_card_balance.csv : 신용카드 월간 잔금을 포함한다.
* previous_application.csv : Home Credit loans의 모든 이전 지원서 정보
* installments_payments.csv :상황 히스토리를 포함한다.
* HomeCredit_columns_description.csv : 데이터 파일의 정보에 대한 묘사를 포함한다.

This file contains descriptions for the columns in the various data files.

### Metric
ROC AUC (Reciever Operating Characteristic Area Under the Curve)를 사용한다.
ROC Curve란 TPR(True Positive Rate)를 y축으로 FPR(False Positive Rate)를 x축으로 하여 서로의 관계를 표현한 그래프이다. TPR은 sensitivity(민감도), recall로도 불리며, 1이라고 예측한 것 중 실제 1인 케이스의 비율을 의미한다(TP/(TP+FN)). FPR은 fall-out으로 불리며, 1-specificity(특이도)로 계산된다.(특이도란 0이라고 예측한 것 중 실제 0인 케이스의 비율이다. 특이도는 TN/(TN+FP)이므로 FPR은 1-TN/(TN+FP)가 된다) naive random guessing model을 의미하는 대각선 보다 위로 갈수록 좋은 모델로 판별한다. AUC란 말그대로 ROC 커브 아래영역의 면적을 구한 것으로 성능이 좋은 모델일 수록 1에 가깝다.


### Exploratory Data Analysis (EDA)
EDA는 통계를 계산하고 데이터의 주요 특성 및 패턴, 데이터 사이의 관계를 분석하는 과정으로 주로 시각적인 방법을 사용한다. high level overview에서 시작해서 특정한 영역으로 좁혀나간다. EDA 조사결과는 모델 선택이나 feature 선택에 도움이 된다.

#### target distribution
imbalanced class problem (http://www.chioka.in/class-imbalance-problem/)을 가진다. 불균형 문제를 반영하기 위해 class 를 weight주는 방식을 사용할 수 있다.

#### missing value
missing value를 채우거나, XGBoost처럼 missing value를 핸들링할 필요없는 모델을 사용할 수 있다. 혹은 missing value가 많은 column을 drop할 수도 있다.

#### Encoding Categorical Variables
LightGBM과 같이 categorical variables을 다룰 수 있는 모델도 있지만, 그렇지 않은 경우에는 Label Encoding이나 One-hot Enconding을 거친다. Label encoding을 할 경우 value상의 관계를 나타낼 수 없으므로 3개 이상의 카테고리를 가질 경우 one-hot encoding을 사용한다. (인코딩에 대한 토론은 https://datascience.stackexchange.com/questions/9443/when-to-use-one-hot-encoding-vs-labelencoder-vs-dictvectorizor 에서 확인할 수 있다.) one-hot encoding의 단점은 카테고리가 엄청나게 증가할 수 있다는 점이다. 따라서 PCA나 dimentionality reduction methods로 dimention의 수를 줄이는 작업이 필요할 수 있다.

#### aligning training and testing data
training data에 없는 categorical variable이 tesing data에 있을 수도 있으므로 이를 처리해 주어야한다.

#### anomalies
측정 오류 등으로 잘못된 값의 데이터가 있을 수 있다. 이 데이터를 missing value로 바꾸거나 np.nan(not a number)값으로 바꿀 수 있다. 특정한 값의 이상 데이터를 가질 경우 이를 boolean columns으로 표시할 수도 있다.

#### correlations
feature와 target의 correlations을 확인하여 데이터를 이해할 수 있다.
http://www.statstutor.ac.uk/resources/uploaded/pearsons.pdf 에 따르면 다음과 같이 해석한다.
* .00-.19 “very weak”
* 20-.39 “weak”
* 40-.59 “moderate”
* 60-.79 “strong”
* 80-1.0 “very strong”

#### Pairs Plot
Pairs Plot은 variables의 multiple pair의 관계를 확인할 수 있는 툴이다.
