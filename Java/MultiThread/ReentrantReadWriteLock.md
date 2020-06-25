### 1.ReentrantReadWriteLock与ReentrantLock区别

* 1. ReentrantLock是排他锁。同一时间只有一个线程在执行ReentrantLock.lock 方法后边的任务。保证了实例变量的线程安全性，但效率低下
  2. ReentrantReadWriteLock，在某些不需要操作实例变量的方法中，可以加快效率
  3. ReentrantReadWriteLock 有读锁和写锁。读锁与读锁不互斥，读锁与写锁互斥，写锁与写锁互斥

  ```java
  public void read() {
      lock.readLock().lock();
      System.out.println(Thread.currentThread().getName() + "start");
      try {
          Thread.sleep(1000);
      } catch (InterruptedException e) {
          e.printStackTrace();
      }
      System.out.println(Thread.currentThread().getName() + "end");
  
      lock.readLock().unlock();
  }
  
  public void write() {
      lock.writeLock().lock();
      System.out.println(Thread.currentThread().getName() + "start");
      try {
          Thread.sleep(1000);
      } catch (InterruptedException e) {
          e.printStackTrace();
      }
      System.out.println(Thread.currentThread().getName() + "end");
  
      lock.writeLock().unlock();
  }
  ```