# 工厂模式

**工厂模式（[简单工厂模式](#简单工厂模式)、[工厂模式](#工厂模式)、[抽象工厂模式](#抽象工厂模式)）**

工厂模式关键点是描述好两个角色（工厂，产品）之间的关系

### 简单工厂模式

```java

//定义小汽车接口
public interface ICar {}    

//定义高、中、低档具体的小汽车
public class UpCar implements ICar {}   //高档小汽车
public class MidCar implements ICar {}    //中档小汽车
public class DnCar implements ICar {}   //低档小汽车

//简单工厂
public class CarSimpleFactory {
    public static final String UPTYPE="uptype";
    public static final String MIDTYPE="midtype";
    public static final String DNTYPE="dntype";
    
    public static ICar create(String mark){
        ICar obj = null;
        if(mark.equals(UPTYPE)){
            obj = new UpCar();
        }else if(mark.equals(MIDTYPE)){
            obj = new MidCar();
        }else if(mark.equals(DNTYPE)){
            obj = new DnCar();
        }
        return obj;
    }
}

//测试程序
public class CarTest {
    public static void main(String[] args){
        ICar obj = CarSimpleFactory.create("UPTYPE");
    }
}

/**
* 功能类编制步骤
*   定制抽象产品接口
*   定制具体产品子类
*   定制工厂类
*/

/**
*   简单工厂 也称 静态工厂
*   
*   简单工厂缺陷：不便扩展
*   如：当增加新类型时，需要增加新子类，并需要修改工厂类的create()方法
* 
*/

```

### 工厂模式

应用场景：

当用户需要一个类的子类实例，但不希望与该子类形成耦合 或 不知道该类有哪些子类可用时
    

```java

//定义小汽车接口
public interface ICar {}    

//定义高、中、低档具体的小汽车
public class UpCar implements ICar {}   //高档小汽车
public class MidCar implements ICar {}    //中档小汽车
public class DnCar implements ICar {}   //低档小汽车

//定义抽象工厂
public abstract class AbstractFactory {
    public abstract ICar create();
}

//定义高档小汽车厂
public class UpFactory extends AbstractFactory {
    public ICar create(){
        return new UpCar();
    }
}

//定义中档小汽车厂
public class MidFactory extends AbstractFactory {
    public ICar create(){
        return new MidCar();
    }
}

//定义低档小汽车厂
public class DnFactory extends AbstractFactory {
    public ICar create(){
        return new DnCar();
    }
}

//测试类
public class CarTest {
    public static void mian(String[] args){
        AbstractFactory obj = new UpFactory();
        ICar car = obj.create();
    }
}

/**
* 功能类编制步骤
*   定制抽象产品接口
*   定制具体产品子类
*   定制抽象工厂类 或 接口
*   定制具体工厂子类
*/

```


### 抽象工厂模式

应用场景：

当用户需要系统提供多个对象，但希望和创建对象的类解耦合时
    
```java

//定义小汽车接口，高、中、低档具体的小汽车
public interface ICar {}    
public class UpCar implements ICar {}   
public class MidCar implements ICar {}    
public class DnCar implements ICar {}   

//定义公共汽车接口，高、中、低档公共汽车类
public interface IBus {}
public class UpBus implements IBus {}
public class MidBus implements IBus {}
public class DnBus implements IBus {}

//定义抽象工厂
public abstract class AbstractFactory {
    public abstract ICar create();
    public abstract IBus create();
}

//定义高档工厂类
public class UpFactory extends AbstractFactory {
    public ICar create(){
        return new UpCar();
    }
    public IBus create(){
        return new UpIBus();
    }
}

//定义中档工厂类
public class MidFactory extends AbstractFactory {
    public ICar create(){
        return new MidCar();
    }
     public IBus create(){
        return new MidIBus();
    }
}

//定义低档工厂类
public class DnFactory extends AbstractFactory {
    public ICar create(){
        return new DnCar();
    }
    public IBus create(){
        return new DnIBus();
    }
}

//测试类
public class CarTest {
    public static void mian(String[] args){
        AbstractFactory obj = new UpFactory();
        ICar car = obj.create();
    }
}

/**
* 功能类编制步骤
*   定制抽象产品接口
*   定制具体产品子类
*   定制抽象工厂类 或 接口
*   定制具体工厂子类
*/

//注：工厂模式 与 抽象工厂模式，是统一的，只是抽象工厂模式是多产品系列

```
    
## 应用示例





