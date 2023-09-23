---
layout: post
title:  "Python Queue task_done() 과 join()"
date:   2022-11-28 21:03:36 +0530
categories: Python
---

## task_done()
* #### Queue.task_done()
앞서 큐에 넣은 작업이 완료되었음을 나타냄

큐 consumer thread에서 사용된다.
작업을 꺼내는 데 사용되는 get()마다, 후속 task_done() 호출은 작업에 대한 처리가 완료되었음을 큐에 알려준다.
join()이 블로킹 중이면, 모든 항목이 처리되면 (큐로 put()된 모든 항목에 대해 task_Done() 호출이 수신되었음을 뜻함) 재개된다.

큐에 있는 항목보다 더 많이 호출되면 ValueError 를 발생시킨다.

## join()
* #### Queue.join()
큐의 모든 항목을 꺼내서 처리할 때까지 블록한다.

완료되지 않은 작업 카운트는 항목이 큐에 추가될 때마다 올라간다.
consumer thread가 task_done() 을 호출해서 항목을 꺼내고 작업이 모두 완료되었음을 나태낼 때마다 카운트가 내려간다.
완료되지 않은 작업 카운트가 0으로 떨어지면, join()은 블록 해제된다.

---
참고자료
[markyang92 - [python] queue](https://velog.io/@markyang92/python-queue-stack#queuefull)