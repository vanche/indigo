---
title: "[TF] Autograph"
layout: post
headerImage: false
category: blog
author: huiwon
---

### 텐서플로우의 Autograph로 파이썬 코드를 TF graph로 변환가능해지다.
7월 18일, 텐서플로우에서 새로운 feature인 AutoGraph를 소개하였다.[^1] eager excution을 사용하지 않는다면, 그래프를 생성하고 후에 그래프를 실행시키는 방식으로 프로그램을 작성해야 했다. graph는 removing common sub-expressions과 fusing kernel 같은 최적화를 위해, 그리고 플랫폼에 상관없이 분산 학습과 배포를 간단히 하게 위해 필요했다. eager excution 으로도 파이썬의 naive feature룰 이용가능하지만, 파이썬 인터프리터에 오버헤드를 주거나 최적화에 적합하지 않았다. tf.cond 등을 이용하는 방법이 있지만 실제로 사용해 본다면 굉장히 불편함을 느낄 것이다. 앞으로는 autograph.convert()를 함수위에 decorate하기만 하면된다. Autograph가 자동으로 graph-ready code를 생성할 것이다. if / while과 같은 control flow를 사용하기 쉬워진다. 더구나 nested contorl flow와 array append, break / continue / print / assert 같은 constructs도 지원한다.

### AutoGraph 와 Eager excution
tf.contrib.eager.defun로 여전히 graph excution이 가능하다. 향후엔 AutoGarph가 defun과 매끄럽게 통합될 것이다.

[^1]: 이 글은  텐서플로우 포스팅 https://medium.com/tensorflow/autograph-converts-python-into-tensorflow-graphs-b2a871f87ec7 을 정리한 글입니다.
