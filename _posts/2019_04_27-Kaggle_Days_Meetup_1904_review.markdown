---
title: "Kaggle Days Meetup - Seoul (2019) 참가 후기"
layout: post
headerImage: false
category: blog
author: huiwon
---
4월 18일에 kaggle days meeptup이 서울에서 열렸다 :)! 한국에 몇 명 없는 캐글 마스터 중 한 분인 명대우님의 경험을 공유하는 세션이 있어서 꼭 참석하고 싶었는데, 다행히 선발 인원에 들어갔다. 앞으로 이런 자리가 자주 열리고, 한국에서 캐글이 더 많이 보편화되었으면 좋겠다. ㅎㅎ  
정해진 일정은 아래와 같았다.  
<br>  

## 스케쥴
- 19:00 ~ 19:20 : Welcome Speech from Kaggle Days (영상통화)
- 19:20 ~ 19:50 : 스폰서 스피치 (삼성 SDS 분석사례 공유, 고락윤님)
- 20:00 ~ 21:00 : 키노트 스피치 (Kaggle Master 명대우님)
- 21:10 ~ 22:00 : 관심주제 별 네트워킹 (with 피자&맥주)  

행사는 SDS잠실 사옥에서 열렸는데, 동관을 찾기가 쉽지않아 헤맸었다. ㅠㅠ.. 그래서 앞부분은 좀 놓쳤지만, 다행히 키노트 스피치는 처음부터 끝까지 들을 수 있었다! 이 포스팅은 주로 키노트 스피치를 들으면서 정리한 내용을 공유할 목적으로 작성되었다.  
<br>  


## 키노트 스피치 (Kaggle Master 명대우님)
명대우님은 "End-to-End, Raw Input, Single Model"이 정석이라고 생각하신다고 했다. 이 말이 내가 추구하는 방향과도 비슷해서 공감이 많이 되었다. 특히 좋은 single model을 설계하는게 매우 중요하다고 생각하기 때문이다. (물론 kaggle에선 ensemble없이는 최상위권에 진입하기 힘들긴하다.ㅠ)  
### 뒤늦게 competition에 참여하는 경우
요즘은 active 대회가 많기 때문에 뒤늦게 시작하는 경우가 많은데, 이럴경우 most vote나 best score kernel을, 그중에서도 single model을 선택하여 시작하는 걸 추천하셨다. 그 다음 discussion에서 score boost가 있는 항목(ex: external data, different losses, different input generator, post processing, CV, ensemble 등)들을 찾아 구현한다. (나도 뒤늦게 대회를 시작할 경우 이 과정을 거치는 편이다. 특히 discussion에는 중요한 힌트들이 곳곳에 숨어있기에 계속 주시할 필요가 있다.)  

### 정석적인 프로세스
그러나 위에는 여유가 없을 때의 작전이고, 정석대로 풀어서 상위권에 올라가고 싶을 때 해야할 것은 다음과 같다고 하셨다.  
* 적당한 파이프라인 빨리 찾기:
  image size : 128 /224  
  simple augmentation : flip, rotate 90  
  use small size back bone : mobilenetV2, SEResNet을 자주 사용  
* 적당한 파이프라인을 찾았다면 한번에 하나씩 가능한 많은 실험을 트라이하는것이 좋다. 스코어 부스트가 있는 항목들을 찾아야한다. 어떤 실험을 했고, 점수가 얼마나 올랐는지 다 기록을 정리해야한다. 가지고 있는 리소스와 시간 동안 어떤 실험을 할 것인지 기획하고, 실행, 기록해야한다. 이 때 리소스가 정말 많이 필요하다. 그래서 딥러닝 컴패티션은 금수저가 유리한 대회라고 하셨는데, 이 부분 진심으로 공감한다. ㅠㅠ... 돈많이 벌어서 멀티 타이탄gpu로 돌려보고싶은데, 언제쯤 꿈을 이루려나...  
* 인풋 제너레이터: 인식하는데 필요한 정보를 input generator에 모두 포함될 수 있도록 해야한다. 예제로 doodle competition을 드셨다. 이 competiton은 캐치마인드처럼 사람이 그린 그림을 토대로 label을 분류해야하 하는 대회다. 내 첫 kaggle image 대회였는데, 아쉽게도 메달을 못 따고 고배를 마셨다.ㅠㅠ 그 때, 내 파이프라인이 흑백의 완성된 이미지를 이용하여 모델을 학습하는 거였는데, 가능한 많은 정보를 모델에 주는것이 꽤 크리티컬했다는걸 나중에 알게되었다. 명대우님은 사람이 일부를 그려도 정답을 알 수 있는 경우가 있기 때문에, 라인의 길이, 그리는 속도, 가속도 등을 같이 인코딩하였다고 하셨다. 이렇게 해ㅆ을 때 모델이 더 많은 정보를 얻을 수 있다. 그 다음 예제는 "Human Protein Atlas Image Classification"로 도메인 지식이 중요한 대회였다. 이 대회 같은 경우 이미지 사이즈가 매우 중요한 문제였기 때문에, input generator에 image shape를 반영하였다고 한다. augmentation에 도움이 되는 오픈소스로 https://github.com/aleju/imgaug 를 추천해 주셨다. 물론 data augmentation이 무조건 좋은 것은 아니다. 어떤 대회에서는 rotate, flip vertical 까진 효과가 있으나 flip horizon과 brightness등은 효과를 못 보셨다고 한다. competitiona data 에 따라 일반적이지 않은 augmentation 방법들도 존재한다고 한다. 예를 들어 배를 segmentation하는 문제인데, 퍼즐처럼 맞춰셔 새로운 이미지를 만들 수 있다. 또한 audio관련 대회인 경우는 소리를 섞거나 작은소리, 큰소리등으로 변형할 수 있다. TTA(Test Time Augmentation)처럼 여러번 prediction후, 그것의 average등을 사용할 수 있다. different input size를 활용할 수도 있고, temperature scaling 후 평균을 활용할 수도 있다.  
* 대우님은 flatten을 잘 안 쓰고 GAP(Global Averaging Pooling)을 주로 사용하신다고 한다. 특히 GAP의 경우 이미지 사이즈에 영향을 받지 않는다.  
* 도메인 지식이 있다면, 모델이 필요없는 정보를 배우지 않도록, 데이터가 특정 방향으로 치우치지 않도록 하는게 좋다.  
* 일반적으로 train data의 각 channel의 mean, std를 활용하고, 각 픽셀의 mean, std를 활용하기도 한다.
* validation split. Random하게 뽑아도 valid score와 public LB가 같은 방향으로 움직인다면 OK다. 라벨 종류가 많지 않다면 public LB Proving, test data 분포에 맞춰 나눠야 한다. segmentation이라면 오브젝트/배경 비율로 배분할 수 있다. multi label task라면 multi label을 고려한 split을 할 수 있다. data size가 작아서 리소스가 충분하다면 Cross Validataion을 시도 할 수도 있다. 그러나 보통은 리소스가 부족하므로 앙상블한 모델에 서로 다른 validataion set을 사용하는 편이다.  
* Back bone 모델 선정 : 대부분의 캐글 경쟁은 데이터가 이미지셋만큼 충분하지 않아서 pretrained model로 transfer learning필수라고 말하셨다. (이 부분은 생각차가 있는것같다. 왜냐면 캐글 그랜드 마스터 중 한명인 Pavel Ostyakov의 가이드를 들은 적이 있는데, 자신은 간단한 모델에서 스크래치부터 학습시키는 편이라고 했다.) 대우님은, 1 epoch정도씩만 학습 후, validation score기준으로 back bone을 선정한다고 하셨다.   
* back size는 크면 클수록 좋다. class가 많거나 multi class task이거나 할수록 유효하다.  
* 트릭1 : 1차 트레인 후 레이어의 90퍼 정도를 프리즈하고 뒤쪽 레이어만 추가 학습시키는 방법을 사용할 수 있다.  
* 트릭2 : gradient accumulate하는 optimizer를 사용한다.  
* 요즘 선호하는 모델방식은 multi inputs, multi losses로 data에서 배울 수 있는 정보를 최대한 모델에 제공하는 것이다. 예로 segmentation loss와 classification loss를 같이 사용하거나, image retrieval 대회에서 이미지들이 같은 이미지인지 구분하기 위해 마지막 이미지 벡터같의 거리에 triplet loss를 사용할 수 있다고 한다. (triple loss+CCE)  
* 딥러닝할 때 스태킹을 잘 쓰지 않으나, 간혹 그랜드마스터들의 솔루션에서 스태킹을 사용하는 경우가 있다고 한다.  
* 상관도에 따라 앙상블을 조정한다. 앙상블할 때 여유가 있다면, 파이프라인이나 인풋을 다르게 하 수 있다. 예를들어 스피치 데이터에서 채널을 쌓아서 인풋을 만들기도 하셨다고 한다. back bone모델을 다르게 사용하는 방법도 있다.  
* submission을 선택할 때 보통은 2개를 선택하도록 되어있는데, 이 때 CV LB가 경향성이 같으면 신경을 안써도 된다. 만약 경향이 다르다면 CV, LB 가 max인 것을 각각 하나씩 선택한다.  
* train/test set의 라벨 분포가 다른지 확인할 수 있다. 라벨 수가 많다면 train set에서 가장 분포가 큰 몇 개만 확인할 수도 있다. 클래수 수가 많지 않다면 서브미션을 만들어 competition초기에 제출해서 public 분포를 확인하는 방법도 있다.  

말씀하시길 이상향은 "End-to-End, Raw Input, Single Model"이지만, 스코어를 올리기 위해 다른 트릭이나 팁을 사용하게 된다고 하시며 끝마치셨다. ㅋㅋ 정말 공감간다.ㅋㅋ

## 후기  
이후 시간은 자신이 관심있는 주제별로, 네트워킹을 가지는 시간이었는데, 이미 시간이 너무 늦어서 나는 참석하지 못하였다. 주제가 주로 도메인으로 나눠져 있었는데, 캐글 밋업인 만큼 대회관련된 주제였으면 어땠을까 싶다. 키노트는 정말 알찬 내용이 가득했다. 정말 아낌없이 공유해주신걸 느꼈다. ㅠㅠ ㅎㅎ 앞으로도 이런 밋업이 자주 열렸으면 좋겠다!
