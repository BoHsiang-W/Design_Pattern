## no proxy
* 被追求者(姓名) <- 追求者 (送娃,送花,送巧克力)

```java
class Pursuit{
    private SchoolGirl schoolGirl;
    public Pursuit(SchoolGirl schoolGirl) {
        this.schoolGirl = schoolGirl;
    }
    public void giveChocolate() {
        System.out.println("送" + schoolGirl.getName() + "巧克力");
    }
    public void giveFlowers() {
        System.out.println("送" + schoolGirl.getName() + "花");
    }
    public void giveDolls() {
        System.out.println("送" + schoolGirl.getName() + "洋娃娃");
    }
}

class SchoolGirl{
    private String name;
    public SchoolGirl(String name) {
        this.name = name;
    }
    public String getName() {
        return name;
    }
}

SchoolGirl schoolGirl = new SchoolGirl("张三");
schoolGirl.getName(); // 张三

Pursuit pursuit = new Pursuit(schoolGirl);
pursuit.giveChocolate(); // 送张三巧克力
pursuit.giveFlowers(); // 送张三花
pursuit.giveDolls(); // 送张三洋娃娃
```
## proxy
* 被追求者(姓名) <- 代理者 (送娃,送花,送巧克力)

```java
class Proxy{
    private SchoolGirl schoolGirl;
    public Proxy(SchoolGirl schoolGirl) {
        this.schoolGirl = schoolGirl;
    }
    public void giveChocolate() {
        System.out.println("送" + schoolGirl.getName() + "巧克力");
    }
    public void giveFlowers() {
        System.out.println("送" + schoolGirl.getName() + "花");
    }
    public void giveDolls() {
        System.out.println("送" + schoolGirl.getName() + "洋娃娃");
    }
}

SchoolGirl schoolGirl = new SchoolGirl("张三");
schoolGirl.getName(); // 张三

Proxy proxy = new Proxy(schoolGirl);
proxy.giveChocolate(); // 送张三巧克力
proxy.giveFlowers(); // 送张三花
proxy.giveDolls(); // 送张三洋娃娃
```
## proxy with pursuit
* interface (送禮物)
    ├─ 追求者 (送娃, 送花, 送巧克力)
    └─ 代理者 (送娃, 送花, 送巧克力)
           ↓
      被追求者 (姓名)

```java 
interface IGiveGift {
    void giveChocolate();
    void giveFlowers();
    void giveDolls();
}
class Pursuit implements IGiveGift {
    private SchoolGirl schoolGirl;

    public Pursuit(SchoolGirl schoolGirl) {
        this.schoolGirl = schoolGirl;
    }

    public void giveChocolate() {
        System.out.println("送" + schoolGirl.getName() + "巧克力");
    }

    public void giveFlowers() {
        System.out.println("送" + schoolGirl.getName() + "花");
    }

    public void giveDolls() {
        System.out.println("送" + schoolGirl.getName() + "洋娃娃");
    }
}

class Proxy implements IGiveGift {
    private Pursuit pursuit;

    public Proxy(SchoolGirl schoolGirl) {
        this.pursuit = new Pursuit(schoolGirl);
    }

    public void giveChocolate() {
        this.pursuit.giveChocolate();
    }
    public void giveFlowers() {
        this.pursuit.giveFlowers();
    }
    public void giveDolls() {
        this.pursuit.giveDolls();
    }
}

SchoolGirl schoolGirl = new SchoolGirl("张三");
schoolGirl.getName(); // 张三

Proxy proxy = new Proxy(schoolGirl);
proxy.giveChocolate(); // 送张三巧克力
proxy.giveFlowers(); // 送张三花
proxy.giveDolls(); // 送张三洋娃娃
```

## Proxy 模式
* 代理模式（Proxy Pattern）為其他物件提供一種代理以控制對這個物件的存取

```java
interface ISubject {
    void request();
}

class RealSubject implements ISubject {
    public void request() {
        System.out.println("真實請求");
    }
}

class Proxy implements ISubject {
    private RealSubject realSubject;

    public Proxy() {
        this.realSubject = new RealSubject();
    }

    public void request() {
        this.realSubject.request();
    }
}

Proxy proxy = new Proxy();
proxy.request(); // 真實請求
```

### 代理模式的應用
* 遠端代理：為一個物件在不同的位址空間提供局部代表，這樣可以隱藏物件存在於不同位址空間的事實。
* 虛擬代理：根據需要建立消耗很大的物件，透過它來存放實體化需要很長時間的真實物件。
* 安全代理：用來控制真實物件存取的許可權。
* 智慧代理：只有在呼叫真實物件時，代理處理另一些事