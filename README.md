# apache-airflow

##airflow的守护进程

airflow系统在运行时有许多守护进程，它们提供了airflow的全部功能。守护进程包括WEB服务器：webserver、调度程序：scheduler、执行单元：worker、消息队列监控工具：Flower等。


###webserver

webserver是一个守护进程，它接受HTTP请求、允许通过其它WEB应用程序与airflow进行交互，webserver提供以下功能：

- 中止、恢复、触发任务  
- 监控正在运行的任务，断电续跑任务
- 执行ad-hoc命令或SQL语句来查询任务的状态，日志等详细信息
- 配置连接，包括不限于数据库、ssh的连接等

webserver守护进程使用gunicorn服务器处理并发请求，可通过修改{AIRFLOW_HOME}/airflow.cfg文件中workers的值来控制处理并发请求的进程数。
例如：

```python
workers = 4 # 表示开启4个gunicoen worker处理web请求
```

启动webserver守护进程：
```shell
airflow webserver -D
```

###scheduler

scheduler是一个守护进程，它周期性轮询任务的调度计划，以确定是否触发任务执行。

启动scheduler守护进程：

```shell
airflow scheduler -D
```

###worker

worker是一个守护进程，它启动一个或多个celery任务队列，负责执行具体的DAG任务。

当设置airflow的executors设置为CeleryExecutor时才需要开启worker守护进程。

启动worker守护进程，默认的队列名为default：

```shell
airflow worker -D
```

###flower

flower是一个守护进程，可用于通过web页面监控celery消息队列。

启动flower守护进程：

```shell
airflow flower -D
```

默认的端口为5555，可以在浏览器通过地址"http://localhost:5555"来访问flower，对celery消息队列进行监控。


