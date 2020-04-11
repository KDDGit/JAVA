# 工厂模式

**工厂模式（[简单工厂模式](简单工厂模式)、[工厂模式](工厂模式)、[抽象工厂模式](抽象工厂模式)）**

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

```

### 工厂模式

应用场景：
    
    当用户需要一个类的子类实例，但不希望与该子类形成耦合 或 不知道该类有哪些子类可用时
    



### 抽象工厂模式

应用场景：

    当用户需要系统提供多个对象，但希望和创建对象的类解耦合时
