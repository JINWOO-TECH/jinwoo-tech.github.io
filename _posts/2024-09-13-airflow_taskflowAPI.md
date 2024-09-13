---
layout: post
title:  "Airflow taskflow API"
categories: data_engineering
tags: Airflow
description: Airflow taskflow API
---


<h2>
    <span class = "jjw_h2_style">Airflow taskflow API </span>
</h2>

taskFlow API는 기존의 Operator 기반 방식보다 더 직관적이고 코드 내에서 워크플로우의 흐름을 쉽게 표현할 수 있습니다.
저는 현업에서 주로 taskflow API로 airflow를 활용했습니다!

### `TaskFlow API의 장점`
1. **Python 함수로 작업 정의**: TaskFlow API는 Python 함수를 사용하여 작업을 정의할 수 있습니다. 이는 기존의 Operator 방식에 비해 더 직관적이며, 파이썬 언어만으로 작업의 입력, 출력, 상태 관리를 할 수 있어 코드 작성과 유지보수가 훨씬 용이합니다.

2. **데코레이터(@task) 기반의 간결한 코드**: TaskFlow API는 작업을 정의할 때 @task 데코레이터를 사용합니다. 덕분에 복잡한 코드 없이 간결하게 태스크를 정의할 수 있습니다.

3. **자동 입력/출력 관리**: TaskFlow API는 태스크 간의 입력과 출력을 자동으로 관리해주기 때문에, 각각의 태스크가 의존하는 데이터를 쉽게 전달할 수 있습니다.

4. **XComs 관리의 단순화**: TaskFlow API는 내부적으로 XCom을 사용하여 데이터를 주고받지만, 이를 사용자 코드에서 명시적으로 관리할 필요 없이 쉽게 사용할 수 있습니다.


~~~python
from airflow.decorators import dag, task

# DAG 정의
default_args = {
    'owner': 'jinwoo',
    'start_date': datetime(2023, 4, 21, tzinfo=local_tz),
    'provide_context': False,
}

@dag(
    schedule_interval="30 4 * * *",
    default_args=default_args,
    catchup=True,
    max_active_runs=1,
)
def example_taskflow_api():
    
    # Task 1: 데이터 추출
    @task()
    def extract():
        data = {"key1": 1, "key2": 2, "key3": 3}
        return data
    
    # Task 2: 데이터 변환
    @task()
    def transform(data: dict):
        transformed_data = {k: v * 10 for k, v in data.items()}
        return transformed_data
    
    # Task 3: 데이터 로드
    @task()
    def load(data: dict):
        print(f"Loading data: {data}")
    
    # Task 연결
    extracted_data = extract()
    transformed_data = transform(extracted_data)
    load(transformed_data)

# DAG 인스턴스 생성
example_dag = example_taskflow_api()

~~~