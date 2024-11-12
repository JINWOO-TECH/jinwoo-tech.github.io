---
layout: post
title:  "(1) Airflow의 스케줄링과 Execution Date"
categories: data_engineering
tags: Airflow
description: Airflow의 스케줄링(backfill,catch up)과 Execution Date
---


<h2>
    <span class = "jjw_h2_style">1. Airflow의 스케줄링 </span>
</h2>

<p>각 DAG에 대한 스케줄 간격을 정의하여 파이프라인이 실행되는 시간을 결정할 수 있습니다.</p>

<p>
    DAG의 스케줄을 잡기 위해서는 <span class="jjw_line">start_date</span> (필수), 
    <span class="jjw_line">schedule_interval</span> (필수), 
    <span class="jjw_line">end_date</span> 등을 설정해야 합니다.
</p>

<p>
    먼저 해당 DAG이 언제부터 시작할지를 정해야 합니다. 
    <span class="jjw_line">(start_date 설정)</span>
</p>

<p>
    이후 <span class="jjw_line">schedule_interval</span>을 설정해서 실행 간격을 정합니다.
    만약 스케줄이 특정 날짜까지만 실행되기를 원하면, 
    <span class="jjw_line">end_date</span>를 설정하면 됩니다.
</p>

<p>
    <span class="jjw_line"><strong>start_date + schedule_interval</strong> 때 첫 실행</span>이 일어납니다.
</p>

<p>
    <span class="jjw_line">schedule_interval</span>은 Preset을 이용하거나 Cron을 이용해 설정할 수 있습니다.
    주로 Cron 방식으로 표현됩니다.
</p>


<span class="jjw_line">schedule_interval</span> 은 Preset을 이용하거나 cron을 이용해 설정할 수 있습니다.
주로 cron 방식으로 표현합니다.  

<h3>Preset </h3>

<table class="jjw_table">
  <thead>
    <tr>
      <th>Preset</th>
      <th>의미</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>@once</td>
      <td>한 번 실행</td>
    </tr>
    <tr>
      <td>@hourly</td>
      <td>한 시간에 한 번</td>
    </tr>
    <tr>
      <td>@daily</td>
      <td>하루에 한번, 자정에 실행</td>
    </tr>
    <tr>
      <td>@weekly</td>
      <td>일주일에 한 번, 일요일 자정에 실행</td>
    </tr>
    <tr>
      <td>@monthly</td>
      <td>한 달에 한 번, 그 달의 첫 날 자정에 실행</td>
    </tr>
    <tr>
      <td>@yearly</td>
      <td>1년에 한번씩 1월 1일 자정에 실행</td>
    </tr>
  </tbody>
</table>


<h3>cron  </h3>
'* * * * * *' <br>
'분 시간 일 월 요일(0~6)'
<table class="jjw_table">
  <thead>
    <tr>
      <th>Cron 표현식</th>
      <th>의미</th>
      <th>예시</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>* * * * *</td>
      <td>매 분마다 실행</td>
      <td>Every minute</td>
    </tr>
    <tr>
      <td>0 * * * *</td>
      <td>매 시간 정각에 실행</td>
      <td>Hourly at minute 0</td>
    </tr>
    <tr>
      <td>0 0 * * *</td>
      <td>매일 자정에 실행</td>
      <td>Daily at midnight</td>
    </tr>
    <tr>
      <td>0 0 * * 0</td>
      <td>매주 일요일 자정에 실행</td>
      <td>Weekly on Sunday at midnight</td>
    </tr>
    <tr>
      <td>0 0 1 * *</td>
      <td>매달 첫째 날 자정에 실행</td>
      <td>Monthly on the 1st at midnight</td>
    </tr>
    <tr>
      <td>0 0 1 1 *</td>
      <td>매년 1월 1일 자정에 실행</td>
      <td>Yearly on January 1st at midnight</td>
    </tr>
  </tbody>
</table>

예시) 
~~~python
# 저는 taskflow API를 사용했습니다
# taskflow는 간단하게 데코레이터를 사용해 DAG와 Task를 구성하는 방식입니다.

# 매일 04시30 분 실행
default_args = {
    'owner': 'jinwoo',
    'start_date': datetime(2023, 11, 7, tzinfo=local_tz),  # 시작 날짜
}
@dag(
    schedule_interval="30 4 * * *",
    default_args=default_args,
)

# trigger시 실행
default_args = {
    'owner': 'jinwoo',
    'start_date': datetime(2023, 11, 7, tzinfo=local_tz),  # 시작 날짜
}
@dag(
    schedule_interval= None,
    default_args=default_args,
)

~~~

<br>

<h2>
    <span class = "jjw_h2_style">2. backfill, catch up 사용하기 </span>
</h2>

<p>
Airflow를 사용하다 보면, 재실행하거나 현재 시점보다 과거의 배치 작업을 실행해야 할 때, 실패 등의 이유로 특정 작업만 재실행하고 싶을 경우가 발생한다.
이 경우 catch up, backfill, clear를 적절하게 사용하면 된다
</p>

### `catch up`

python 코드로 dag을 작성할 때 dag 속성 안에 catch up이라는 인수를 둘 수 있습니다. 기본값을 False이고 True를 주게 되면 활성화가 됩니다.
catch up은 `start_date ~ 현재(또는 end_date)` 을 순차적으로 실행시킬때 사용합니다. 
예를 들면 `start_date = 2024-08-02` 이고 현재 날짜(또는 end_date)가 `2024-08-05`이고 schedule_interval이 매일 새벽 3시라면
2일부터 5일까지 순차적으로 실행시킵니다.

<br>

#### `catch up 주의 사항` 
catch up이 실행될 때 한번에 실행되기 때문에 서버에 과부하가 올 수 있습니다. 따라서 `max_active_runs`으로 한번에 실행되는 DAGrun의 수를 설정할 수 있습니다

~~~python
default_args = {
    'owner': 'jinwoo',
    'start_date': datetime(2023, 11, 7, tzinfo=local_tz),  # 시작 날짜
}

@dag(
    schedule_interval="30 4 * * *",
    default_args=default_args,
    max_active_runs=1, # 한번에 실행되는 DAGrun 수 설정
    catchup=True,  # 반복시 설정
)
~~~

<br>

### `backfill`
Backfill은 DAG가 이미 배포되어 실행 중일 때, DAG의 시작 날짜 이전의 데이터를 처리하길 원할 때 사용합니다. CLI를 사용하여 backfill을 사용할 수 있습니다


~~~bash
airflow dags backfill [-h] [-c CONF] [--delay-on-limit DELAY_ON_LIMIT] [-x]
                      [-n] [-e END_DATE] [-i] [-I] [-l] [-m] [--pool POOL]
                      [--rerun-failed-tasks] [--reset-dagruns] [-B]
                      [-s START_DATE] [-S SUBDIR] [-t TASK_REGEX] [-v] [-y]
                      dag_id
                   
# 예시
airflow dags backfill -s 2024-09-01 -e 2024-09-02 example_dag
~~~

<table class="jjw_table">
  <thead>
    <tr>
      <th>인수</th>
      <th>설명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>dag_id</td>
      <td>dag의 id</td>
    </tr>
    <tr>
      <td>-c</td>
      <td>Dagrun의 conf 속성을 피클</td>
    </tr>
    <tr>
      <td>--continue-on-failure</td>
      <td>설정하면 일부 작업이 실패해도 백필 계속 진행</td>
    </tr>
    <tr>
      <td>--delay-on-limit</td>
      <td>dag 실행을 다시 실행하기 전에 최대 활성 Dag실행 제한에 도달했을 때 대기하는 시간</td>
    </tr>
    <tr>
      <td>--end-date</td>
      <td>end-date yyyy-mm-dd</td>
    </tr>
    <tr>
      <td>-i</td>
      <td>업스트림 작업을 건너뛰고 정규표현식과 일치하는 작업만 실행</td>
    </tr>
    <tr>
      <td>-I</td>
      <td>dependencies_on_past 속성 무시</td>
    </tr>
    <tr>
      <td>-l</td>
      <td>LocalExecutor로 실행</td>
    </tr>
    <tr>
      <td>-m</td>
      <td>작업을 실행하지 않고 성공한 것으로 표시</td>
    </tr>
    <tr>
      <td>--pool</td>
      <td>사용할 리소스 풀</td>
    </tr>
    <tr>
      <td>--verbose</td>
      <td>더 자세한 로깅 출력 만들기</td>
    </tr>
    <tr>
      <td>-s</td>
      <td>start_date yyyy-mm-dd로 정의</td>
    </tr>
    <tr>
      <td>--reset-dagruns</td>
      <td>설정된 경우 백필은 기존 백필 관련 DAG 실행을 삭제하고 새로 시작</td>
    </tr>
    <tr>
      <td>--rerun-failed-tasks</td>
      <td>설정된 경우 백필은 예외를 throw하는 대신 백필 날짜 범위에 대해 실패한 모든 작업을 자동으로 다시 실행</td>
    </tr>
  </tbody>
</table>





<h2>
    <span class = "jjw_h2_style">3. execution Date </span>
</h2>

Airflow에서 `execution_date`는 DAG이 실행되어야 하는 **기대값**이라고 생각할 수 있습니다. 

예를 들어 매일 새벽 5시에 전날 발생했던 로그들을 S3에 이관하는 작업이 있다고 가정해보겠습니다.  
이때 실제로 `2024-08-23 05:00:00`에 실행된 DAG은 **2024-08-22**의 로그들을 가져옵니다. 

보통 사용자는 오늘 날짜에서 **-1일** 해서 데이터를 가져올 수 있는데, Airflow에서는 `execution_date`를 사용하여 이를 대신할 수 있습니다.

---

### 자주 묻는 질문

처음 Airflow를 이용하는 분들이 종종 이런 질문을 합니다:

> "그러면 그냥 오늘 날짜(`datetime.today().date()`)에서 **-1일** 해서 사용해도 되지 않나요?"

하지만 **오늘 날짜**로 조건을 설정하고 이틀 뒤나 일주일 뒤에 에러를 발견하고 해당 DAG을 다시 실행하려고 한다면 문제가 발생할 수 있습니다.  
이 경우 **재실행하는 지금의 날짜**가 조건으로 설정되기 때문에, 원하는 데이터를 가져오지 못할 가능성이 큽니다.

---


### `execution_date`의 장점

`execution_date`를 사용하면, 재실행하더라도 `execution_date`는 **처음 실행할 때 할당된 값**에서 변경되지 않습니다.  
따라서 backfill이나 DAG 재실행 시 매우 유용하게 사용할 수 있습니다. 
~~~python
# 현업에서 사용했던 execution_date 활용
from airflow.operators.python import get_current_context
from dateutil.relativedelta import relativedelta

def get_date():
    context = get_current_context()
    date = context["data_interval_start"] + relativedelta(hours=9) # UTC -> KST
    today = date.strftime("%Y-%m-%d")
    print(today)
    return today
~~~