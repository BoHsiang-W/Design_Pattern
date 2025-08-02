## 炒股程式
* 客戶
    * 股票1
        * 買入
        * 賣出
    * 股票2
        * 買入
        * 賣出
    * 國債
        * 買入
        * 賣出
    * 房地產
        * 買入
        * 賣出

```java
class Stock1
{
    public void buy() {
        System.out.println("Buying stock at market price");
    }

    public void sell() {
        System.out.println("Selling stock at market price");
    }
}

class Stock2
{
    public void buy() {
        System.out.println("Buying stock2 at market price");
    }

    public void sell() {
        System.out.println("Selling stock2 at market price");
    }
}
class Bond {

    public void buy() {
        System.out.println("Buying bond at market price");
    }

    public void sell() {
        System.out.println("Selling bond at market price");
    }
}
class RealEstate {

    public void buy() {
        System.out.println("Buying real estate at market price");
    }

    public void sell() {
        System.out.println("Selling real estate at market price");
    }
}
```
### client
```java
Stock1 stock1 = new Stock1();
stock1.buy();

Stock2 stock2 = new Stock2();
stock2.buy();

Bond bond = new Bond();
bond.buy();

RealEstate realEstate = new RealEstate();
realEstate.buy();
stock1.sell();
stock2.sell();
bond.sell();
realEstate.sell();
```

## add fund
* 客戶
    * 基金
        * 股票1
        * 股票2
        * 國債
        * 房地產
```java
class Fund {

    Stock1 stock1;
    Stock2 stock2;
    Bond bond;
    RealEstate realEstate;
    public Fund() {
        stock1 = new Stock1();
        stock2 = new Stock2();
        bond = new Bond();
        realEstate = new RealEstate();
    }
    public void buy() {
        stock1.buy();
        stock2.buy();
        bond.buy();
        realEstate.buy();
    }
    public void sell() {
        stock1.sell();
        stock2.sell();
        bond.sell();
        realEstate.sell();
    }
}

Fund fund = new Fund();
fund.buy();
fund.sell();
```
## 面板模式
* 面板模式為子系統中的一組介面提供一個一致的介面，此模式定義了一個高層介面，使得這一子系統更加容易使用

* 客戶
    * 面板1
        * subsystem1
            * 功能1
        * subsystem2
            * 功能2
        * subsystem3
            * 功能3
        * subsystem4
            * 功能4


```java
class Subsystem1 {
    public void feature1() {
        System.out.println("Subsystem1 Feature1");
    }
}

class Subsystem2 {
    public void feature2() {
        System.out.println("Subsystem2 Feature2");
    }
}

class Subsystem3 {
    public void feature3() {
        System.out.println("Subsystem3 Feature3");
    }
}

class Subsystem4 {
    public void feature4() {
        System.out.println("Subsystem4 Feature4");
    }
}

class Panel {
    private Subsystem1 subsystem1;
    private Subsystem2 subsystem2;
    private Subsystem3 subsystem3;
    private Subsystem4 subsystem4;

    public Panel() {
        subsystem1 = new Subsystem1();
        subsystem2 = new Subsystem2();
        subsystem3 = new Subsystem3();
        subsystem4 = new Subsystem4();
    }

    public void method1() {
        subsystem1.feature1();
        subsystem2.feature2();
        subsystem3.feature3();
        subsystem4.feature4();
    }
    public void method2() {
        subsystem1.feature1();
        subsystem2.feature2();
    }
}
```
### client
```java
Panel panel = new Panel();
panel.method1();
panel.method2();
```

## 何時使用面板模式
* 首先在設計初期階段應該要有意識地將不同的兩個層分離
* 層與層之間建立外觀Facade
* 在開發階段子系統往往因為不斷的重構演化而變得越來越複雜
* 增加外觀facade可以提供一個簡單的介面，減少它們之間的依賴
* 在維護一個遺留的大型系統時，可能這個系統已經非常難以維護和擴充了
* 為新系統開發一個外觀facade類別，來提供設計粗糙或高度複雜的遺留程式的比較清晰簡單的介面，讓新系統與Facade物件互動，Facade與遺留程式互動，所有複雜的工作