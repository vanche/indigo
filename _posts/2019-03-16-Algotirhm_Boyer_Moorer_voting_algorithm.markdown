---
title: "[Algorithm] Boyer-Moore majority vote algorithm"
layout: post
headerImage: false
category: blog
author: huiwon
---
sequence 에서 majority variable(등장횟수가 (n+1)/2이상인 변수)을 찾을 때, hash를 이용하면 시간복잡도 O(n), 공간복잡도 O(n)으로 풀 수 있다. 이 공간복잡도를 줄일 수 있을까?   [Boyer-Moore majority vote algorithm](https://en.wikipedia.org/wiki/Boyer%E2%80%93Moore_majority_vote_algorithm)을 사용하면, 공간복잡도 O(1)로 줄일 수 있다. 보이어-무어 알고리즘은 majority variable이 (n+1)/2 이상 등장한다는 특징을 이용한다. 다음은 wikipedia의 pseudo code이다.
```
Initialize an element m and a counter i with i = 0  
For each element x of the input sequence:  
  If i = 0, then assign m = x and i = 1  
  else if m = x, then assign i = i + 1  
  else assign i = i − 1  
Return m
```
가능한 후보 변수 candidate와 counter 변수 cnt를 정의해보자. 현재 보는 위치에서 candidate가 다시 등장하면 cnt에 1을 더해주고, 다른 variable이 나오면 cnt에서 1을 빼준다. 만약 cnt값이 0이면 candidate는 현재 보는 변수가 된다. 그리고 sequence를 다 보았을때의 candidate가 majority variable이 된다. 관련문제로 leetcode의 https://leetcode.com/problems/majority-element 가 있다.
