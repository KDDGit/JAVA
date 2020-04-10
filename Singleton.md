# 单例模式

单例模式（保证一个类只能有一个实例）

特点：     
        1、只能有一个实例       
        2、必须自己创建唯一实例        
        3、向外提供这个实例      
    
注：**单例模式核心是构造方法私有化**

单例模式创建：[直接实例化](#直接实例化)、[延迟实例化](延迟实例化)

### 直接实例化

``` java 
public class Singleton{
    private Singleton(){}
    private static final Singleton single = new Singleton();
    public static Singleton getInstance(){
        return single;
    }
}
```

### 延迟实例化

```java
    public class Singleton2{
        private Singleton2(){}
        private static Singleton2 single = null;
        public static Singleton2 getInstance(){
            if(single == null){
                single = new Singleton2();
            }
            return single;
        }
    }
    //注：getInstance()方法存在线程安全问题
```

三种解决办法：同步方法、同步部分代码、静态内部类

```java
    //1、同步方法
    public static synchronized Singleton2 getInstance(){
        if(single == null){
            single = new Singleton2();
        }
        return single;
    }
    //2、同步部分代码
    public static Singleton2 getInstance(){
        if(single == null){
            synchronized(Singleton2.class){
                if(single == null){
                    single = new Singleton2();
                }
            }
        }
        return single;
    }
    //相比方法1，方法2提高了运行效率
    //3、静态内部类
    public class singleton3{
        private static class my{
            private static final Singleton3 single = new Singleton3();
        }
        private Singleton3(){
            System.out.println("This is new instance");//测试用
        }
        public static final Singleton3 getInstance(){
            return My.single;
        }
    }
    //利用静态内部类生成单利对象更优
    //测试类
    public class Test{
        public static void main(String[] args){
            Scanner s = new Scanner(System.in);
            s.newLine();
            Singleton3 single = Singleton3.getInstance();
            Singleton3 single2 = Singleton3.getInstance();
        }
    }

```

### 应用实例
    
    
    


        
        


