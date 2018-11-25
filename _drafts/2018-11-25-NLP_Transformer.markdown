---
title: "Transformer: Attention is All You Need 리뷰"
layout: post
headerImage: false
category: blog
author: huiwon
---
이제 NLP task에서 빠질 수 없는 transformer에 대해 정리해본다. 이 논문은 NIPS 2017에 등장하여 MT task에서 SOTA를 찍었고, (한때 QA task인 SQuAD에서 SOTA였던) QANet에서 사용되었으며, 현재 best NLP model이라고 주목받는 Bert의 모듈로도 사용되었다.

### Abstract
기존 번역 모델들은 복잡한 RNN, CNN기반의 인코더-디코더 구조를 가졌다. 가장 성능이 좋은 모델은 attention mechanism으로 인코더 디코더를 연결하는 구조였따. transformer는 CNN, RNN 모듈을 완전히 없애고, 오직 attention mechanism을 기반으로 한다. 실험적으로 transformer는 두 MT task(WMT2014 english-to-german, english-to-franch)에서 우수한 성능을 보였으며 다른 task에서도 보편적으로 좋은 성능을 보였다.

### Instroduction
그동안 RNN 모델은 sequence modeling, transductoin 문제에서 SOTA를 이루었다. machine translation 연구도 recurrent language model과 encoder-decoder구조의 바운더리에서 계속 되었다. 이러한 모델은 이전 hidden state $h_{t-1}$를 고려하여 $h_t$가 결정되고, 본질적으로 병렬적으로 계산이 불가능하게 한다. 최근 연구들에서 factorization tricks나 conditional computation을 어느정도 계산적 효율성을 얻었다. 그러나 sequential computation의 근본적인 제약은 여전히 남아있었다. Attention mechanism은 input, output sequence의 distance의 상관없이 dependecy를 모델링하게 함으로써 다양한 시퀀스 task에서 필수불가결한 요소가 되었다. 그러나 기존 attention구조는 rnn과 결합되어 사용되었다. 이 논문에서는 오직 input output의 global dependency를 그리는데 attention mechanism만을 이용하는 transformer architecture를 제안한다.
