### 1.构建单例模式

* 饿汉模式

* synchronized

  ```java
  public class SingleTest {
      private static SingleTest instance;
  
      public static SingleTest getInstance() {
          if (instance == null) {
              synchronized (SingleTest.class) {
                  if (instance == null) {
                      instance = new SingleTest();
                  }
              }
          }
          return instance;
      }
  }
  ```

  

* 静态内部类

  ```java
  class Single
  {
      private static class SingleHandler
      {
          private static Single instance = new Single();
      }
  
      public static Single getInstance()
      {
          return SingleHandler.instance;
      }
  }
  ```

* 静态代码块

  静态代码块在使用该类的时候就会执行

  ```java
  class SingleTest1 {
      private static SingleTest instance;
  
      static {
          instance = new SingleTest();
      }
  
      public static SingleTest getInstance() {
          return instance;
      }
  }
  ```

  

### 2.单例序列化和反序列化

```java

class Single implements Serializable {
    private static class SingleHandler {
        private static Single instance = new Single();
    }

    public static Single getInstance() {
        return SingleHandler.instance;
    }

    protected Object readResolve() throws ObjectStreamException
    {
        return SingleHandler.instance;
    }
    
    public static void main(String[] args)
    {
        String path = "C:\\Study\\test.txt";
        try {
            ObjectOutputStream stream = new ObjectOutputStream(new FileOutputStream(path));
            stream.writeObject(Single.getInstance());
            stream.close();

            System.out.println(Single.getInstance().hashCode());

            ObjectInputStream inputStream = new ObjectInputStream(new FileInputStream(path));
            Single single = (Single) inputStream.readObject();

            System.out.println(single.hashCode());

        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

