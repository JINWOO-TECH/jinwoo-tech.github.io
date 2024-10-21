---
layout: post
title: (코딩테스트_복기 / python3) - 결합 연산으로 수식의 가능한 결과의 최댓값

description: >
  (코딩테스트_복기 / python3) - 결합 연산으로 수식의 가능한 결과의 최댓값

categories: programming
tags: 코딩테스트_문제_python_메모이제이션
---

<h2>
    <span class = "jjw_h2_style"> 문제 </span>
</h2>
주어진 수식에서 연산자를 통해 가능한 모든 결과를 계산하는 문제입니다. 수식은 숫자와 연산자(+, -)로 이루어져 있습니다.

연산자에 따라 숫자를 결합하여 다양한 결과를 얻을 수 있습니다. 예를 들어, 수식 1 - 3 + 5 - 8이 주어졌을 때, 이 수식을 계산할 수 있는 모든 가능한 결과의 최댓값 구하는 것이 목표입니다.

각 숫자와 연산자를 이용하여 여러 가지 방식으로 수식을 분할하고 결합하여 가능한 결과의 리스트를 반환하도록 solution 함수를 작성하세요.


<br>

<h2>
    <span class = "jjw_h2_style"> 풀이 </span>
</h2>
<br>
메모이제이션 & 분할정복

1. 주어진 `arr` 리스트에서 수식의 숫자와 연산자를 분리하여 조합을 계산합니다. 
2. 재귀적으로 수식을 나누고 각 부분에 대해 가능한 결과를 계산합니다. 이때, 연산자의 위치를 기준으로 수식을 왼쪽과 오른쪽으로 나눕니다. 
3. 왼쪽 부분과 오른쪽 부분의 결과를 각각 계산한 후, 두 부분에서 얻은 결과를 가지고 현재 위치의 연산자를 적용합니다. 
4. 각각의 조합에 대해 결과를 누적하여 모든 가능한 결과를 리스트에 저장합니다. 
5. 수식을 다 나눈 후, 최종 결과 리스트를 반환합니다. 리스트 중 max값을 return합니다.

<br><br>

<h2>
    <span class = "jjw_h2_style"> 코드 </span>
</h2>

~~~python
def operate(a, b, op):
    if op == '+':
        return a + b
    else:
        return a - b


# 메모이제이션을 위한 딕셔너리 추가
memo = {}


def cal(arr):
    # 배열을 튜플로 변환해서 딕셔너리의 키로 사용 가능하게 처리
    key = tuple(arr)


    # 메모이제이션된 결과가 있으면 바로 반환
    if key in memo:
        return memo[key]

    # 숫자 하나만 남았을 때 처리
    if len(arr) == 1:
        return [int(arr[0])]

    result = []

    # 연산자를 기준으로 분할하여 재귀적으로 호출
    for i in range(1, len(arr), 2):  # 연산자는 인덱스가 홀수에 위치
        op = arr[i]
        left = cal(arr[:i])  # 왼쪽 부분 계산
        right = cal(arr[i + 1:])  # 오른쪽 부분 계산

        # 왼쪽과 오른쪽 부분을 각각 연산
        for l in left:
            for r in right:
                result.append(operate(l, r, op))

    # 메모이제이션 저장
    memo[key] = result
    return result


def solution():
    answer = -1
    arr = ["1", "-", "3", "+", "5", "-", "8"]
    print(cal(arr))  # 가능한 모든 결과 출력
    return max(answer)


solution()



~~~









