## ThreadGroup

### 线程组的作用

可以批量管理线程或者线程组

#### 1.创建线程组

```java
public class ThreadGroupTest {
    public static void main(String[] args)
    {
        ThreadGroup group = new ThreadGroup("test");
        Thread thread = new Thread(group, new ThreadA());
        thread.start();
    }
}

class ThreadA implements Runnable {
    @Override
    public void run() {
        System.out.println(Thread.currentThread().getThreadGroup().getName());
    }
}
```

#### 2.批量停止线程

```java
public class ThreadGroupTest {
    public static void main(String[] args) throws InterruptedException {
        ThreadGroup group = new ThreadGroup("test");

        Thread[] threades = new Thread[10];
        for (int i = 0; i < threades.length; i++) {
            threades[i] = new Thread(group, new ThreadA());
            threades[i].start();
        }
        Thread.sleep(10000);
        group.interrupt();
    }
}
```

#### 3.线程组中一个线程报错停止所有线程组

```java
public class ThreadGroupTest {
    public static void main(String[] args) throws InterruptedException {
        MyThreadGroup group = new MyThreadGroup("test");

        Thread[] threades = new Thread[10];
        for (int i = 0; i < threades.length; i++) {
            threades[i] = new Thread(group, new ThreadA(String.valueOf(i)));
            threades[i].start();
        }
        Thread.sleep(1000);
        Thread errorThread = new Thread(group, new ThreadA("a"));
        errorThread.start();
    }
}

class ThreadA implements Runnable {
    private String num;

    public ThreadA(String num) {
        this.num = num;
    }

    @Override
    public void run() {
        Integer.parseInt(num);
        while (!Thread.currentThread().isInterrupted()) {
            System.out.println("loopping");
        }
    }
}

class MyThreadGroup extends ThreadGroup {
    public MyThreadGroup(String name) {
        super(name);
    }

    @Override
    public void uncaughtException(Thread t, Throwable e) {
        super.uncaughtException(t, e);
        System.out.println(e.toString());
        this.interrupt();
    }
}
```