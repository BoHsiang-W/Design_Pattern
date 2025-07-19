## 類別與實例
* 物件是一個由類別所定義的實體，並以一組可辨識的屬性和行為來描述。
* 類別是具有相同的屬性和功能的物件的抽象集合。
    - 類別名稱首字母大寫，多個單字時每個單字的首字母皆需大寫（即 PascalCase 命名法）。
    - 對外公開的方法需使用 `public` 修飾符。
* 實例是一個真實的物件。
* 實例化是建立物件的過程。

## 建構方法
建構方法又叫建構函數，其實就是對類別進行初始化。建構方法與類別名稱相同，無傳回值，也不需要 void，在 new 時候呼叫。
```java
public class Cat{
    private String name = "";

    public Cat(String name) {
        this.name = name;
    }
    public String shout() {
        return "my name is " + name + " meow";
    }
}
public class Test{

    public static void main(String[] args) {
        Cat cat = new Cat("Tom");
        System.out.println(cat.shout());
    }
}
```

## 方法多載
方法多載提供了建立名稱相同的多個方法的能力，但這些方法需使用不同的參數類型。

```java
public class Cat{
    private String name = "";

    public Cat(String name) {
        this.name = name;
    }

    public Cat() {
        this.name = "default";
    }
    public String shout() {
        return "my name is " + name + " meow";
    }
}
```
## 屬性與修飾符號
屬性是一個方法或一對方法 及屬性適合於以類別的方法使用方法呼叫
```java

public class Cat{
    private int shoutCount = 3;
    
    public void setShoutCount(int count) {
        if (count <= 10) {
            this.shoutCount = count;
        }
        else{
            this.shoutCount = 10; 
        }
    }
    public int getShoutCount() {
        return this.shoutCount;
    }
    public String shout() {
        String result = "";
        for (int i = 0; i < this.shoutCount; i++) {
            result += "meow ";
        }
        return "my name is " + name + " " + result;
    }
}
```

## 封裝
好處:
* 良好的封裝能夠減少耦合
* 類別內部的實現可以自由地修改
* 類別具有清晰的對外界面

## 繼承
如果子類別繼承於父類別
* 子類別擁有父類別非private的屬性和功能
* 子類別具有自己的屬性和功能 及子類別可以擴充父類別沒有的屬性和功能
* 子類別還可以自己的方式實現父類別的功能

- Protected 代表繼承時子類別可以對基礎類別有完全存取權
```java
public class Animal {
    protected String name = "";
    
    public Animal(String name) {
        this.name = name;
    }
    public Animal() {
        this.name = "default";
    }

    protected int shoutCount = 3;
    public void setShoutCount(int count) {
        this.shoutCount = count;
    }
    public int getShoutCount() {
        return this.shoutCount;
    }

    public String shout() {
        return "";
    }
}

public class Cat extends Animal {

    public Cat(String name) {
        super(name);
    }

    public Cat() {
        super();
    }

    public String shout() {
        String result = "";
        for (int i = 0; i < this.shoutCount; i++) {
            result += "meow ";
        }
        return "my name is " + name + " " + result;
    }
}
```

## 多形
多型代表不同的物件可以執行相同的動作 彈藥透過他們自己的實現方式來執行
* 子類別以父類別的身分出現
* 子類別在工作時以自己的方式實現
* 子類別以父類別的身分出現時，子類別特有的屬性和方法不可使用

- 子類別可以選擇使用override來覆寫父類別的方法

## 重構

```java
public class Animal {
    protected String name = "";
    
    public Animal(String name) {
        this.name = name;
    }
    public Animal() {
        this.name = "default";
    }

    protected int shoutCount = 3;
    public void setShoutCount(int count) {
        this.shoutCount = 10;
    }
    protected int getShoutCount() {
        return this.shoutCount;
    }

    public String shout() {
        String result = "";
        for (int i = 0; i < this.shoutCount; i++) {
            result += getShoutSound() + " ";
        }
        return "my name is " + name + " " + result;
    }
}
```

## 抽象類別
* 抽象類別不能實例化
* 抽象方法是必須被子類別重寫的方法
* 如果類別中包含抽象方法 那麼類別就必須定義為抽象類別 不論是否還包含其他一般方法

## 介面
* 介面是把隱式公共方法和屬性組合起來以封裝特定功能的集合
* 一旦類別實現了介面 類別就可以支援介面所指定的所有屬性和成員
* 宣告介面在語法上與宣告抽象類別完全相同 但不予許提供介面中任何成員的執行方法
* 一個類別可以支援多個介面 多個類別也可以支援相同的介面
```java
public interface IChange {
    public String changeThing(String thing);
}

public class MachineCat extends Cat implements IChange {
    public MachineCat() {
        super();
    }
    public MachineCat(String name) {
        super(name);
    }

    public String changeThing(String thing) {
        return super.shout() + " and I can change " + thing;
    }
}