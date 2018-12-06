---
title: "Kaggle Doodle Recognition Challenge 참가후기"
layout: post
headerImage: false
category: blog
author: huiwon
---
어제 Kaggle의 "Quick, Draw! Doodle Recognition Challenge" 대회가 막을 내렸다. 약 두달 가까이 진행되었던 컴패티션이었고, 처음으로 끝까지 제대로 붙잡으면서 해봤던 대회였는데, 메달을 못 따서 아쉽게 됐다. 첫 vision관련 대회니까 참가하면서 image classification의 모델과 연구들을 공부하면서 많이 배웠던거에 의의를 두어야하나..(삽질을 너무 많이 해서 아쉽다..ㅠㅠ) 나는 이 대회에서 좀 더 효율적으로 참여했어야 했다....

### 대회 개요
이 대회는 "Quick, Draw!"라는 구글이 제공하는 handwriting sketch 데이터를 이용한 분류문제이다. 데이터셋은 쉽게 말해서 캐치마인드처럼, 사람이 그린 그림과 그에 해당하는 라벨(+기타 시간, 지역 등 다른정보)로 구성되어있다. 평가는 top3 acc로 이루어진다.

### 관련 연구 탐색
처음에는 sketch 특성을 고려한 연구들을 찾아봤다. 다음과 같은 paper들이 있었다.
* [sketch-a-net][1]:sketch의 부분들을 multi channel로 구성하고, multi-scale cnn을 통과시킨후 joint Bayesian fusion으로 앙상블한다.
* [Sequential Dual Deep Learning with Shape and Texture Features for Sketch Recognition][2]: 그림을 t개의 그룹(시간을 기준으로 그때까지 그려진 그림.)으로 나눈 후, texture-feature를 학습할 SAN(sketch-a-net)과 shape-feature(sketch의 geometry 정보)를 학습할 SC-net을 rnn으로 연결시킨 구조를 통과시킨다.
* [Sketch Recognition with Deep Visual-Sequential Fusion Model][3]:그림을 3개의 그룹(각각 60%, 80%, 100% 그려진 그림)으로 나누어 ConvNet을 통과시키고, 이어 visual network(FC,R-FC와 R-LSTM 2layer), fusion layer를 거친다.


[1]:https://arxiv.org/pdf/1501.07873.pdf
[2]:https://arxiv.org/pdf/1708.02716.pdf
[3]:http://www.yugangjiang.info/publication/17MM-Sketch.pdf
