## 裝飾者模式 Example1

* 人
    * 穿大T恤
    * 穿垮褲
    * 穿球鞋
    * 穿西裝
    * 穿皮鞋
    * 打領帶
    * 形象展示

```java
public class Person {
    private String name;

    public Person(String name) {
        this.name = name;
    }

    public void wearTShirt() {
        System.out.println(name + " 穿大T恤");
    }
    public void wearBaggyPants() {
        System.out.println(name + " 穿垮褲");
    }
    public void wearSneakers() {
        System.out.println(name + " 穿球鞋");
    }
    public void wearSuit() {
        System.out.println(name + " 穿西裝");
    }
    public void wearLeatherShoes() {
        System.out.println(name + " 穿皮鞋");
    }
    public void wearTie() {
        System.out.println(name + " 打領帶");
    }
    public void show() {
        System.out.println("裝扮的 " + name);
    }
}
```
```java
Person xc = new Person("小明");
System.out.println("小明的裝扮1：");
xc.wearTShirt();
xc.wearBaggyPants();
xc.wearTie();
xc.show();
System.out.println("小明的裝扮2：");
xc.wearTShirt();
xc.wearBaggyPants();
xc.wearTie();
xc.show();
```


## 裝飾者模式 Example2
* 人
    * 形象展示

* 形象展示
    * 穿大T恤
    * 穿垮褲
    * 穿球鞋
    * 穿西裝
    * 穿皮鞋
    * 打領帶
    
```java
public class Person {
    private String name;

    public Person(String name) {
        this.name = name;
    }

    public void show() {
        System.out.println("裝扮的 " + name);
    }
}

public abstract class Finery {
    public abstract void show();
}

public class TShirt extends Finery {
    
    public void show() {
        System.out.println(" 穿大T恤");
    }
}
public class BaggyPants extends Finery {
    
    public void show() {
        System.out.println(" 穿垮褲");
    }
}
...
```

```java
Person xc = new Person("小明");
System.out.println("小明的裝扮1：");
Finery tShirt = new TShirt();
Finery baggyPants = new BaggyPants();
Finery tie = new Tie();

tShirt.show();
baggyPants.show();
tie.show();
xc.show();

System.out.println("小明的裝扮2：");
tShirt.show();
baggyPants.show();
xc.show();
```

## 裝飾模式
* 裝飾模式 動態地給一個物件增加一些額外的職責，就增加功能來說，裝飾模式比生成子類別更為靈活。

Component：定義一個物件介面，可以給這些物件動態地增加職責。  
ConcreteComponent：定義一個具體的物件，也可以給這個物件增加一些職責。  
Decorator（裝飾抽象類別）：繼承 Component，從外類別來擴充 Component 類別的功能，但對 Component 來說，無需知道 Decorator 的存在。  
ConcreteDecorator：具體的裝飾物件，作用是給 Component 增加職責的功能。

```java
abstract class Component {
    public abstract void operation();
}

class ConcreteComponent extends Component {
    public void operation() {
        System.out.println("執行具體物件的操作");
    }
}

abstract class Decorator extends Component {
    protected Component component;

    public void setDecorator(Component component) {
        this.component = component;
    }

    public void operation() {
        if (component != null) {
            component.operation();
        }
    }
}

class ConcreteDecoratorA extends Decorator {
    private String addedState;

    public void operation() {
        super.operation();
        addedState = "新增狀態";
        System.out.println("ConcreteDecoratorA 的操作，狀態：" + addedState);
    }
}
class ConcreteDecoratorB extends Decorator {

    public void operation() {
        super.operation();
        this.addBehavior();
    }

    private void addBehavior() {
        System.out.println("ConcreteDecoratorB 的操作，行為：新增行為");
    }
}

ConcreteComponent component = new ConcreteComponent();
ConcreteDecoratorA decoratorA = new ConcreteDecoratorA();
ConcreteDecoratorB decoratorB = new ConcreteDecoratorB();

decoratorA.setDecorator(component);
decoratorB.setDecorator(decoratorA);
decoratorB.operation();

```

## 裝飾者模式 Example3
* interface (形象展示)
    * 人 (形象展示)
    * 服飾 (形象展示)
        * 穿大T恤 (形象展示)
        * 穿垮褲 (形象展示)
        * 穿球鞋 (形象展示)
        * 穿西裝 (形象展示)
        * 穿皮鞋 (形象展示)
        * 打領帶 (形象展示)

```java
public interface Icharacter {
    void show();
}

public class Person implements Icharacter {
    private String name;

    public Person(String name) {
        this.name = name;
    }

    public void show() {
        System.out.println("裝扮的 " + name);
    }
}

public abstract class Finery implements Icharacter {
    protected Icharacter component;

    public void decorate(Icharacter component) {
        this.component = component;
    }

    public void show() {
        if (component != null) {
            component.show();
        }
    }
}

public class TShirt extends Finery {
    public void show() {
        super.show();
        System.out.println("穿大T恤");
    }
}
...

Person xc = new Person("小明");

System.out.println("小明的裝扮1：");
TShirt tShirt = new TShirt();
tShirt.decorate(xc);

Sneakers sneakers = new Sneakers();
sneakers.decorate(tShirt);

sneakers.show();

System.out.println("小明的裝扮2：");
BaggyPants baggyPants = new BaggyPants();
baggyPants.decorate(xc);
Tie tie = new Tie();
tie.decorate(baggyPants);
tie.show();

```

## 商場收銀程式
* cashsuper <- CashContext
    * cashnormal
    * cashrebate
    * cashreturn
    * cashreturnrebate

```java
public class CashReturnRebate extends CashSuper {
    private double moneyRebate = 1d;
    private double moneyCondition = 0d;
    private double moneyReturn = 0d;

    public CashReturnRebate(double moneyRebate, double moneyReturn, double moneyCondition) {
        this.moneyRebate = moneyRebate;
        this.moneyReturn = moneyReturn;
        this.moneyCondition = moneyCondition;
    }
    public double acceptCash(double price, int num) {
        double result = price * num * this.moneyRebate;
        if (moneyCondition > 0 && result >= moneyCondition) {
            return result - Math.floor(result / moneyCondition) * moneyReturn;
        }
        return result;
    }
}

public class CashContext {
    private CashSuper cashSuper;

    public CashContext(String type) {
        switch (type) {
            case "1":
                cashSuper = new CashNormal();
                break;
            case "2":
                cashSuper = new CashRebate(0.8d);
                break;
            case "3":
                cashSuper = new CashReturn(0.7d);
                break;
            case "4":
                cashSuper = new CashReturn(300d, 100d);
                break;
            case "5":
                cashSuper = new CashReturnRebate(0.8d, 100d, 300d);
                break;

        }
    }

    public double getResult(double price, int num) {
        return this.cashSuper.acceptCash(price, num);
    }
}
```

### 簡單工廠 + 策略 + 裝飾模式
* interface     ←
    * cashsuper ↑ <- CashContext
        * cashnormal
        * cashrebate
        * cashreturn
* 裝飾模式有一個重要的優點 把類別中的裝飾功能從類別中搬移去除 這樣可以簡化原有的類別

* interface     ←
    * cashsuper ↑ <- CashContext
        * cashrebate
        * cashreturn
    * cashnormal
        
```java
public interface ISale {
    double getResult(double price, int num);
}
public class CashSuper implements ISale {
    protected ISale component;

    public void decorate(ISale component) {
        this.component = component;
    }
    public double acceptCash(double price, int num) {
        var result = 0d;
        if (this.component != null) {
            result = this.component.acceptCash(price, num);
        }
        return result;
    }
}

public class CashNormal extends Isale {
    public double acceptCash(double price, int num) {
        return price * num;
    }
}

public class CashRebate extends CashSuper {
    private double moneyRebate = 1d;

    public CashRebate(double moneyRebate) {
        this.moneyRebate = moneyRebate;
    }

    public double acceptCash(double price, int num) {
        double result = price * num * this.moneyRebate;
        return super.acceptCash(result, 1);
    }
}

public class CashReturn extends CashSuper {
    private double moneyCondition = 0d;
    private double moneyReturn = 0d;

    public CashReturn(double moneyCondition, double moneyReturn) {
        this.moneyCondition = moneyCondition;
        this.moneyReturn = moneyReturn;
    }

    public double acceptCash(double price, int num) {
        double result = price * num;
        if (moneyCondition > 0 && result >= moneyCondition) {
            return super.acceptCash(result - Math.floor(result / moneyCondition) * moneyReturn, 1);
        }
        return super.acceptCash(result, 1);
    }
}

public class CashContext {
    private ISale cs;

    public CashContext(String type) {
        switch (type) {
            case "1":
                this.cs = new CashNormal();
                break;
            case "2":
                this.cs = new CashRebate(0.8d);
                break;
            case "3":
                this.cs = new CashRebate(0.7d);
                break;
            case "4":
                this.cs = new CashReturn(300d, 100d);
                break;
            case "5":
                // 先打8折 再滿300減100
                CashNormal cashNormal = new CashNormal();
                CashReturn cashReturn = new CashReturn(300d, 100d);
                CashRebate cashRebate = new CashRebate(0.8d);
                cashReturn.decorate(cashNormal);
                cashRebate.decorate(cashReturn);
                this.cs = cashRebate;
                break;
            case "6":
                // 先滿200減50 再打7折
                CashNormal cashNormal2 = new CashNormal();
                CashRebate cashRebate2 = new CashRebate(0.7d);
                CashReturn cashReturn2 = new CashReturn(200d, 50d);
                cashReturn2.decorate(cashNormal2);
                cashRebate2.decorate(cashReturn2);
                this.cs = cashRebate2;
                break;
        }
    }

    public double getResult(double price, int num) {
        return this.cs.acceptCash(price, num);
    }
}
```

* 裝飾器的優點是 把類別中的裝飾功能從類別中搬移去除，這樣可以簡化原有的類別。
* 有效地把類別的核心職責和裝飾功能區分開 而且可以去除相關類別中重複的裝飾邏輯