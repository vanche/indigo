---
title: "[ML] 앙상블 기법"
layout: post
headerImage: false
category: blog
author: huiwon
---

### Emsemble Method
가장 좋은 모델 하나보다 다양한 모델의 예측을 수집하여 더 좋은 결과를 얻을 수 있는데 이러한 방법을 앙상블 기법(Emsemble Method)이라한다. 실제 Kaggle 등의 ML Contest에서 좋은 성과를 내는 솔루션들은 앙상블 기법을 이용하는 경우가 많다.  
분류기들의 예측을 모아 다수결로 클래스를 예측하는 방식을 hard voting 분류기라하며, 각 분류기가 weak learner라 하더라도 앙상블 기법에 의해 더 좋은 성능을 낼 수 있다. (그러나 모든 분류기가 서로 독립적이고 오차에 대한 상관관계가 없어야 한다. 따라서 같은 데이터로 학습시킬 경우 정확도가 낮아진다.) 분류기로 클래스의 확률을 예측하여 이를 평균을 내서 가장 높은 확률의 클래스를 예측하는 방법을 soft voting이라고 한다.  
### Bagging과 Pasting
같은 알고리즘으로 데이터 서브셋을 무작위하게 구성하여 학습시키는 방식도 있다. 데이터셋의 중복을 허용하는 방식인 bagging과 중복을 불허하는 방식인 pasting으로 구분할 수 있다. 원본 데이터에서 서브셋을 샘플링한 데이터로 학습시켜 개별예측기는 원본데이터로 학습시킬 때보다 편향되어 있지만, 예측기들로부터 클래스 최빈값을 구하는 수집함수를 통과하면 이러한 편향과 분산은 감소한다. 일반적으로 bagging이 좀 더 좋은 성능을 보인다.  
bagging을 사용하면 어떤 샘플은 전혀 선택되지 않기도 하는데, 이러한 샘플을 oob(out-of-bagging) 샘플이라 한다. 이 샘플들을 이용해 앙상블 메소드의 성능을 평가할 수 있다. 사이킷런런의 BaggingClassifier는 oob_score 파라미터를 True로 세팅하면 앙상블 평가를 제공한다. BaggingClassifier로 특성을 샘플링할 수도 있다. 샘플과 특성 모두 샘플링하는 방식인 Random Patches Method와 특성만을 샘플링하는 Random subspces method가 있다.
### Random Forest
결정 트리에 앙상블을 적용한 것을 랜덤 포레스트라 한다. 랜덤 포레스트로 특성의 중요도를 측정할 수 있다. 노드에 사용된 특성에 따라 불순도가 얼마나 감소하느냐에 따라 중요도가 결정된다. 사이킷런에서 전체 중요도가 1이 되도록 정규화되어 계산된다.
### Boosting
부스팅은 weak learner 여러개를 연결해 strong learner로 만드는 앙상블 기법이다. 부스팅 방법 중 가장 많이 쓰이는 것은 AdaBoost와 Gradient Boost이다. AdaBoost는 잘못 분류된 샘플의 가중치를 높여 새로운 예측기에 학습시키고, 다시 샘플의 가중치를 업데이터하여 다음 예측기가 학습시키는 방식으로 진행하는 방법이다. Gradient Boostring은 예측기의 잔여 오차로 다음 예측기를 학습시킨다
### Stacking (stacked generalization)
스태킹은 모든 예측기의 예측을 취합하는 모델을 학습시키는 것이다. 예측기들의 예측을 취합하여 마지막 예측을 하는 예측기를 blender 또는 meta learner라고 한다. 
