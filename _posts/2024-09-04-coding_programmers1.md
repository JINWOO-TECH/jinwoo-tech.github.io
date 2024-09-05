---
layout: post
title: (프로그래머스 / python3) - [PCCP 기출문제] 1번 / 붕대 감기
description: >
  (프로그래머스 / python3) - [PCCP 기출문제] 1번 / 붕대 감기
categories: programming
tags: 코딩테스트_문제_python_기타
---

<h2>
    <span class = "jjw_h2_style"> 문제 </span>
</h2>



어떤 게임에는 <span style="background-color: yellow;">붕대감기</span> 라는 기술이 있습니다. <br>
<span style="background-color: yellow;">붕대감기</span>는 <span style="background-color: yellow;">t</span>초 연속으로 붕대를 감는데 성공한다면
<span style="background-color: yellow;">y</span> 만큼의 체력을 추가로 회복합니다. 또한 공격을 받지 않는 동안은 초당 <span style="background-color: yellow;">x</span> 만큼 회복합니다 <br>
해당 기술은 bandage = [t,x,y] 로 주어집니다. <br><br>
단 게임 캐릭터에는 최대 체력이 존재해, 최대 체력을 넘는 것은 불가능합니다. 최대 체력은 health로 주어집니다. <br>
기술을 쓰는 도중 몬스터에게 공격을 당하면 기술이 취소되고, 공격을 당하는 순간에는 체력을 회복할 수 없습니다. 
<span style="background-color: yellow;">a</span> 시간에 <span style="background-color: yellow;">b</span> 만큼 피해를 줍니다. <br>
공격에 대한 정보는 attacks = [[a,b]]로 주어집니다. <br><br>
현재 체력이 0이하가 된다면 -1을 return하고 아닐 경우 모든 공격이 끝난 직후 남은 체력을 return 하세요. <br><br>

[문제 풀러 가기](https://school.programmers.co.kr/learn/courses/30/lessons/250137)
<br><br>

<h2>
    <span class = "jjw_h2_style"> 풀이 </span>
</h2>
시물레이션 문제

1. attacks의 공격 정보가 담긴 만큼 실행
2. 이전 공격 시간을 저장해서 치유가 되는 시간 구하기  
3. 치유가 되는 시간을 가지고 추가 회복량 구하기
4. 공격 받기전 치유량을 더하기
5. 피해량 계산
6. health값이 0보다 작으면 return -1
7. 아니면 끝까지 실행후 health값 return

<br><br>

<h2>
    <span class = "jjw_h2_style"> 코드 </span>
</h2>

~~~python
def solution(bandage, health, attacks):
    init_health = health
    # 시전시간
    bandage_time = bandage[0]
    # 초당회복
    bandage_recover = bandage[1]  
    # 추가회복
    bandage_add_recover = bandage[2]  
    
    # 이전 공격 시간 초기화
    prev_attact_time = 0
    
    for attack in attacks:
        atttack_time = attack[0]
        damgage = attack[1]
        
        # 이전 공격과 지금 공격까지 흐른 시간 
        elapsed_time = atttack_time - prev_attact_time 
        
        # 공격받기 직전까지 회복 가능
        now_bandage_time = elapsed_time - 1 
        
        # 이전 공격 시간 재설정 
        prev_attact_time = atttack_time
        
        # 추가 회복 횟수 구하기
        add_recover_count = now_bandage_time // bandage_time
        recover = now_bandage_time * bandage_recover  + (add_recover_count * bandage_add_recover)
        health += recover
        if health > init_health:
            health = init_health
         
        health -= damgage
        if health <= 0:
            return -1
        
    return health

~~~









