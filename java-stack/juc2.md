---
title: JUC-线程池面试题hot
categories:
  - 技术栈
  
abbrlink: 2701
date: 2025.5.22
tags: 
   - java 
   - juc
   - 面试
---

# 线程池

## 1.线程池用过没有？ 说说他的核心参数？

线程在正常执行或者异常中断时会被销毁，如果频繁的创建很多线程，不仅会消耗系统资源，还会降低系统的稳定性，一不小心把系统搞崩了。

使用线程池可以带来以下几个好处：

- 线程池内部的线程数是可控的，可以灵活的设置参数；
- 线程池内会保留部分线程，当提交新的任务可以直接运行；
- 方便内部线程资源的管理，调优和监控；

**为了减少频繁的创建线程和销毁线程带来的性能损耗**

首先，所有任务的调度都是由**execute**方法完成的，这部分完成的工作是：检查现在线程池的**运行状态、运行线程数、运行策略**，决定接下来执行的流程，是直接申请线程执行，或是缓冲到队列中执行，亦或是直接拒绝该任务。其执行过程如下：



首先检测线程池运行状态，如果不是**RUNNING**，则直接拒绝，线程池要保证在RUNNING的状态下执行任务。

如果workerCount < corePoolSize，则创建并启动一个线程来执行新提交的任务。

如果workerCount >= corePoolSize，且线程池内的**阻塞队列**未满，则将任务添加到该阻塞队列中。

如果workerCount >= corePoolSize && workerCount < maximumPoolSize，且线程池内的阻塞队列已满，则**创建并启动一个线程**来执行新提交的任务。

如果workerCount >= maximumPoolSize，并且线程池内的阻塞队列已满, 则根据**拒绝策略**来处理该任务, **默认的处理方式是直接抛异常。**

七大核心参数：

**corePoolSize**：线程池核心线程数量，如果设置为5，线程池初始化后默认保持5个线程待命。默认情况下，线程池中线程的数量如果 <= corePoolSize，那么即使这些线程处于空闲状态，那也不会被销毁。

**maximumPoolSize**：线程池允许的最大线程数。当任务队列已满，且当前线程数小于 `maximumPoolSize` 时，线程池会创建新的线程来处理任务，直至线程数达到 `maximumPoolSize`。

**keepAliveTime**：当线程池中线程的数量大于corePoolSize，并且某个线程的空闲时间超过了keepAliveTime，那么这个线程就会被销毁。

**unit**：就是keepAliveTime时间的单位。

**workQueue**：工作队列。当没有空闲的线程执行新任务时，该任务就会被放入工作队列中，等待执行。

**threadFactory**：用于创建线程的工厂。通过自定义线程工厂，你可以为线程设置名称、优先级等属性。

**handler**：拒绝策略。当一个新任务交给线程池，如果此时线程池中有空闲的线程，就会直接执行，如果没有空闲的线程，就会将该任务加入到阻塞队列中，如果阻塞队列满了，就会创建一个新线程，从阻塞队列头部取出一个任务来执行，并将新任务加入到阻塞队列末尾。如果当前线程池中线程的数量等于maximumPoolSize，就不会创建新线程，就会去执行拒绝策略

## 2.线程池的拒绝策略

常用的四种拒绝策略包括：CallerRunsPolicy、AbortPolicy、DiscardPolicy、DiscardOldestPolicy，此外，还可以通过实现**RejectedExecutionHandler**接口来自定义拒绝策略。

CallerRunsPolicy，使用线程池的调用者所在的线程去执行被拒绝的任务，除非线程池被停止或者线程池的任务队列已有空缺。

**AbortPolicy，直接抛出一个任务被线程池拒绝的异常。**这个是默认的

DiscardPolicy，不做任何处理，静默拒绝提交的任务。

DiscardOldestPolicy，抛弃最老的任务，然后执行该任务。

自定义拒绝策略，通过实现接口可以自定义任务拒绝策略。

**RejectedExecutionHandler实现**

