## 1.ReentrantLock使用

```java
ReentrantLock lock = new ReentrantLock();
public void add()
{
    lock.lock();
    try
    {
        Thread.sleep(1000);
    }
    catch (Exception ex)
    {
        ex.printStackTrace();
    }
    finally {
        lock.unlock();
    }
}
```

## 2.ReentrantLock使用Condition实现通知/等待

* ReentrantLock与synchronized区别
  	1.	ReentrantLock可以借助于Condition实现类似synchronized的wait，notify/notifyAll功能
   	2.	在lock对象中可以创建多个Condition(对象监视器)，线程对象可以注册在指定的Condition中，从而可以实现有选择性的线程通知，调度线程更加灵活
   	3.	notify/notifyAll方法，被通知的线程是JVM随机选择的
   	4.	synchronized相当于整个lock对象中只有一个Condition，所有线程都注册在该线程上。notifyAll会通知所有waiting线程，没有选择权，效率较低。
   	5.	Condition 中的signal/signalAll 相当于object类中的notify/notifyAll方法。

```java
ReentrantLock lock = new ReentrantLock();
Condition condition = lock.newCondition();

public void add()
{
    lock.lock();
    try
    {
        System.out.println(Thread.currentThread().getName() + "start...");
        condition.await();
        System.out.println(Thread.currentThread().getName() + "end...");
    }
    catch (Exception ex)
    {
        ex.printStackTrace();
    }
    finally {
        lock.unlock();
    }
}

public void single()
{
    lock.lock();
    try
    {
        condition.signal();
    }
    finally {
        lock.unlock();
    }
}
```

### 3.公平锁和非公平锁

* 公平锁：线程获得锁的顺序是按照线程加锁的顺序来分配，先来先得的FIFO先进先出顺序。
* 非公平锁：随机获得锁，先来到不一定先获得锁

```java
ReentrantLock lock = new ReentrantLock(false);//非公平锁
```

