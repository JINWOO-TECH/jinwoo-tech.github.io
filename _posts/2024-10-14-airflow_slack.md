---
layout: post
title:  "(5) Airflow Slack 알람"
categories: data_engineering
tags: Airflow
description: Airflow Slack 알람
---
> aws 프리티어로 airflow 사용하시는 분들은 아마 webserver와 scheduler 두 개를 띄우면 서버가 터질겁니다.... <br>
> 그래서 저는 webserver는 내렸리고 scheduler만 실행한 상태에서 진행했습니다. <br>
> 자세한 airflow CLI는 따로 포스팅 하겠습니다!


<h2>
    <span class = "jjw_h2_style">STEP 1.slack app 만들기 </span>
</h2>
<br>

[slack app 만들기](https://api.slack.com/apps)

your app > create new app > From scratch

![Xixia](/assets/images/dataengineer/20241014slackcreateapp1.png)

<br>

![Xixia](/assets/images/dataengineer/20241014slackcreateapp2.png)
>`App Name` : 원하는 이름 <br>
> `Pick a workspace to develop your app in` : 원하는 workspace

<br>

<h2>
    <span class = "jjw_h2_style">STEP 2. app 설정 </span>
</h2>

Incoming Webhooks > Activate Incoming Webhooks (ON) > Add New Webhook to Workspace
<br>
your app > create new app > From scratch
![Xixia](/assets/images/dataengineer/20241014slackcreateapp3.png)

<br>

원하는 채널 설정 > 허용 클릭
![Xixia](/assets/images/dataengineer/20241014slackcreateapp4.png)


<br>

Incoming Webhooks > Webhook URLs for Your Workspace에 지정한 슬랙 채널에 메시지를 보낼 수 있는 url과 토큰이 부여됩니다. <br>
> ex)https://hooks.slack.com/services/`{your-token}` 

![Xixia](/assets/images/dataengineer/20241014slackcreateapp5.png)

<br>

서버에서 test 방법
> `Sample curl request to post to a channel`의 `copy`를 클릭합니다. <br>
> 서버에서 copy한 명령어 실행

![Xixia](/assets/images/dataengineer/20241014slackcreateapp6.png)

<br>

<h2>
    <span class = "jjw_h2_style">STEP 3. airflow ui에서 connections 설정 </span>
</h2>
<br>
Admin > Connections > `+` 버튼 클릭
![Xixia](/assets/images/dataengineer/20241014slackcreateapp7.png)

<br>

![Xixia](/assets/images/dataengineer/20241014slackcreateapp8.png)
> `Connection Id` : 원하는 id <br>
> `Connection Type` : Slack Incoming Webhook <br>
> `Slack Webhook Endpoint` : https://hooks.slack.com/services/ <br>
> `Webhook Token` : webhook 토큰

<br>

<h2>
    <span class = "jjw_h2_style">STEP 4. dag 생성 </span>
</h2>

<br>

일단 저는 webhook과 연결되는`slack_alert.py` 와 task의 error를 일으키는 `error_call.py`를 만들었습니다. <br>
만약 slack에 전달되는 메세지를 바꾸고 싶으시면 `slack_alert.py`의 `message` 부분을 수정하면 됩니다.


`dags/slack_alert.py`
~~~python
from airflow.providers.slack.operators.slack_webhook import SlackWebhookOperator

class SlackAlert:
 def __init__(self, channel):
     self.slack_channel = channel


 def slack_fail_alert(self, context):
     alert = SlackWebhookOperator(
             task_id='slack_failed',
             slack_webhook_conn_id='slack',
             message="""
             :red_circle: Task Failed.
             *Task*: {task}
             *Dag*: {dag}
             *Execution Time*: {exec_date}
             *Log Url*: {log_url}
             """.format(
                 task=context.get('task_instance').task_id,
                 dag=context.get('task_instance').dag_id,
                 exec_date=context.get('logical_date'),
                 log_url=context.get('task_instance').log_url,
                 )
             )
     return alert.execute(context=context)

~~~

dags/error_call.py
~~~python
"""
24/10/14
error_call test
"""
from datetime import datetime, timedelta
import pendulum
from airflow.decorators import dag, task
from slack_alert import SlackAlert

local_tz = pendulum.timezone("Asia/Seoul")
alert = SlackAlert('airflow_monitoring')
default_args = {
 'owner': 'jinwoo',
 'start_date': datetime(2022, 3, 1, tzinfo=local_tz),
 'provide_context': False,
 "on_failure_callback": alert.slack_fail_alert
}
# -----------------------------------------------------------#
# ------------------------------------------------------------#
@dag(
 schedule_interval=None,
 default_args=default_args,
 catchup=False,
 tags=['error_call', 'test']
)
def error_call_v1():
 @task
 def test():
     print(1 / 0)

 test()
error_call_v1 = error_call_v1()

~~~

slack에 error 전달
![Xixia](/assets/images/dataengineer/20241014slackcreateapp9.png)