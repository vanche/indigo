---
title: "2017 Google I/O 참가 후기 (1)"
layout: post
headerImage: false
category: blog
author: huiwon
---

## Google I/O 2017 에 다녀오다

2017.5.17 ~ 2017.5.19 3일동안 마운틴뷰에서 진행된 구글 아이오 컨퍼런스에 다녀왔다. 컨퍼런스 참가권을 어떻게 얻을 수 있었는지부터 컨퍼런스에서 어떤 것을 경험할 수 있었는지 기록으로 남기고 공유하기 위해 글을 쓰게 되었다.

 

 

 
## Google Code Jam to I/O for Women

2014년부터 구글에서는 여성을 대상으로 하여, 상위 150여명에게 구글 I/O에 초대하는 Google Code Jam to I/O for Women을 개최한다. 취지는 기술 분야에서 여성들이 좀 더 활동하도록 격려하는 것이다. (아래 인용문은 Google Code Jam to I/O 페이지의 안내문으로, 지금은 페이지가 업데이트되서 찾을 수 없어 다른 사이트에서 인용된 글귀를 퍼왔다.)

>At Google, we’re proud of the fact that every day, women are making technology that changes the world. We believe that by creating the right environments and programs, women in tech will be better positioned to continue to drive transformational change in the industry and beyond. We are committed to bringing more women together in the online contest space and to our annual developer conference, Google I/O. And that’s why, for the fourth year, we are hosting Code Jam to I/O for Women as one way to do just that.

CS든 PS든 여성이 소수라 이런 기회를 줌으로써, 여성의 참여가 활성화되도록 권장하는 경우가 종종 있는데 모두가 동의하는 건 아닌가보다. 코드포스 사이트에서는 이러한 대회가 남성에게 불공평하다는 글과 댓글에 토론이 이어지기도 했다. 이 포스트의 목적은 이 대회가 공평하냐 아니냐에 대해 쓰기 위해서는 아니므로 이런 논란에 대해선 넘어가겠다. 어쨌든 이 대회에서 우승하면(상위권에 들면) 초대권과 약간의 경비[^1]를 지원받는다. 미국거주자가 아니면 자비를 꽤 들여야 하지만, 개인이 부담하기에 참가권이 매우 비싼만큼 PS를 좋아하고 I/O에 관심이 있는 여자 개발자라면 이러한 기회를 살리기를 꼭 추천한다!
[^1]: 2017년 기준 최대 500달러까지 여행경비를 돌려받을 수 있다. 구글이 제시하는 절차를 따라야 한다.

나는 이 대회가 있다는 것을 같이 PS를 해왔던 친구에게서 작년에 처음 들었는데, 작년에는 입사시기와 업무 문제로 컨퍼런스에 다녀오지 않았다. 그 당시에 했던 업무도 좋은 경험이었지만, 올해만은 구글 아이오에 참가해보고 싶었고, 리프레쉬도 하고 싶어서 PS를 같이 준비했던 또 다른 친구와 코드잼을 준비하였다. 현역때와는 다르게 PS에 시간을 많이 못 쓴 것이 꽤 큰 걱정이었다. 그래서 주말엔 틈틈히 코드잼 기출문제를...

#### Code Jam Contest
Google Code Jam to I/O는 기본적으로 Google Code Jam의 룰을 따른다. 3~4문제정도로 이루어지고, 문제당 small, large case가 존재하며, 각 문제별 포인트와 패널티를 합하여 등수가 정해진다. 다른 점은 온라인 라운드 한번으로 끝난다는 것 정도이다.
난이도는 매해 비슷하므로 기출문제를 풀면 어느정도의 수준으로 나오는지 짐작이 가능하다. 내 체감난이도는 Code Jam Round 1 > > > Code Jam to I/O >= Code Jam Qualificiation Round 이다. Code Jam Qual라운드랑 비슷한 난이도지만, 기회가 한번뿐이고 A,B,C의 small, large 케이스중 하나라도 틀린다면 순위에 타격이 크기때문에 긴장감이 꽤 크다.
150분이라는 길지 않은 시간동안 진행되므로 Large케이스를 통과하는 solution을 먼저 생각하여 프로그래밍하는게, small -> large 로 각각의 solution을 구현하여 제출하는것보다 났다.(물론 large가 확실하지 않다면 small이라도 확실하게 구현하는것이 좋다.) 물론 각자의 방식에 따라 다를 수도 있다. 어쨌든 제출 시간 패널티가 있으므로 개인의 판단하에 어떤것이 자신에게 더 좋고 순위에 이득인지 전략을 미리 고민해 보는것이 좋다.



