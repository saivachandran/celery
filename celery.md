Celery
-------

```
Celery is a task queue implementation for Python web applications used to asynchronously execute work outside the HTTP request-response cycle.
```

# What's the difference between Celeryd and Celerybeat
```
Celery can be used to run batch jobs in the background on a regular schedule. A key concept in Celery is the difference between the Celery daemon (celeryd), which executes tasks, Celerybeat, which is a scheduler. Think of Celeryd as a tunnel-vision set of one or more workers that handle whatever tasks you put in front of them. Each worker will perform a task and when the task is completed will pick up the next one. The cycle will repeat continously, only waiting idly when there are no more tasks to put in front of them.

Celerybeat on the other hand is like a boss who keeps track of when tasks should be executed. Your application can tell Celerybeat to execute a task at time intervals, such as every 5 seconds or once a week. Celerybeat can also be instructed to run tasks on a specific date or time, such as 5:03pm every Sunday. When the interval or specific time is hit, Celerybeat will hand the job over to Celeryd to execute on the next available worker.
```

Architecture
------------
```
The internal working of Celery can easily be stated as the Producer/Consumer model. Producers place the jobs in a queue, and consumers are ready them from the queue. Given this, at high-level Celery has 3 main components:
Producers – are commonly the ‘web nodes’, the web service process, handling the web request. During the request processing, tasks are delegated to Celery i.e. pushed into the task queue.
Queue – it is the broker, which basically helps passing tasks from web application to Celery worker(s). Celery has full support for RabbitMQ and Redis, also supports Amazon SQS and Zookeeper but with limited capabilities (RabbitMQ docs).
Consumers – consumers are ‘worker nodes’, listening to queue head, whenever a task is published, they consume and execute it. Workers can also publish back to the queue, triggering another tasks, hence they can also behave as producers.
```

Producer-Consumer setup
-------------------------
```
The most basic setup is to have both producer (web/REST service), and consumer (workers) running on the same machine, and all the code and configuration in a single repository. The web service gets the user requests, place the tasks in the queue to be processed asynchronously by the worker(s), running independently of the web service. This separation of logic and process, makes it highly scaleable, we can have web service and task workers running separately on different nodes.
```
Celery Architecture
-------------------


