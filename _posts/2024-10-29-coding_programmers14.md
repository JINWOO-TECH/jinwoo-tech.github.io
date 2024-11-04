---
layout: post
title: (프로그래머스 / python3) - 거스름돈

description: >
  (프로그래머스 / python3) - 거스름돈

categories: programming
tags: 코딩테스트_문제_python_dp
---

<h2>
    <span class = "jjw_h2_style"> 문제 </span>
</h2>
Finn은 편의점에서 야간 아르바이트를 하고 있습니다. 야간에 손님이 너무 없어 심심한 Finn은 손님들께 거스름돈을 n 원을 줄 때 방법의 경우의 수를 구하기로 하였습니다.

예를 들어서 손님께 5원을 거슬러 줘야 하고 1원, 2원, 5원이 있다면 다음과 같이 4가지 방법으로 5원을 거슬러 줄 수 있습니다.

1원을 5개 사용해서 거슬러 준다.
1원을 3개 사용하고, 2원을 1개 사용해서 거슬러 준다.
1원을 1개 사용하고, 2원을 2개 사용해서 거슬러 준다.
5원을 1개 사용해서 거슬러 준다.
거슬러 줘야 하는 금액 n과 Finn이 현재 보유하고 있는 돈의 종류 money가 매개변수로 주어질 때, Finn이 n 원을 거슬러 줄 방법의 수를 return 하도록 solution 함수를 완성해 주세요.

[문제 풀러 가기](https://school.programmers.co.kr/learn/courses/30/lessons/12907)

<br>

<h2>
    <span class = "jjw_h2_style"> 풀이 </span>
</h2>
<br>

1. DP 배열 초기화:
   * DP 배열 dp를 길이 n + 1만큼 생성하고, dp[0]을 1로 설정합니다.
   * dp[0]을 1로 두는 이유는, 0원을 만드는 경우의 수는 1가지로 생각하기 때문입니다.

2. 각 동전별로 경우의 수 계산:
   * 보유하고 있는 동전 종류별로 반복문을 돌며, 특정 동전을 사용할 때 가능한 경우의 수를 누적해서 계산합니다.
   * 예를 들어, 1원짜리 동전부터 시작하면 dp[1], dp[2]…를 각각 1원짜리 동전 하나를 사용해 만들 수 있는 경우의 수로 업데이트합니다.

3. 동전을 활용해 가능한 금액 계산:
   * 각 동전 m을 기준으로, m원부터 n원까지 순회하며 dp[price]를 업데이트합니다. 여기서 dp[price] += dp[price - m]을 수행하여, price 금액을 만들 수 있는 경우의 수를 계속 누적합니다.
   *예를 들어, 현재 동전이 2원일 때 price가 4원이라면, dp[4]는 2원을 만들 수 있는 방법이 추가되므로 dp[4]에 dp[4 - 2]을 더하여 값을 누적합니다.

4. 최종 답 구하기:
   * 최종적으로 dp[n]에는 n원을 만드는 모든 경우의 수가 저장됩니다.
   * 문제에서 큰 수 처리를 위해 답을 1000000007로 나눈 나머지를 반환하도록 합니다
   
<br><br>

<h2>
    <span class = "jjw_h2_style"> 코드 </span>
</h2>

~~~python
def solution(n, money):
    answer = 0
    
    dp = [0] * (n + 1)
    
    dp[0] = 1
    
    for m in money:
        for price in range(m, n+1):
            dp[price] += dp[price - m]
    
    return dp[-1] % 1000000007


~~~









