## 工廠方法模式實現

```java
public interface IFactory {
    public Operation createOperation();
}

public class AddFactory implements IFactory {
    public Operation createOperation() {
        return new AddOperation();
    }
}

public class SubFactory implements IFactory {
    public Operation createOperation() {
        return new SubOperation();
    }
}

public class MulFactory implements IFactory {
    public Operation createOperation() {
        return new MulOperation();
    }
}

public class DivFactory implements IFactory {
    public Operation createOperation() {
        return new DivOperation();
    }
}

public class OperationFactory {
    public static Operation createOperation(String type) {
        IFactory factory = null;
        Operation operation = null;
        switch (type) {
            case "add":
                factory = new AddFactory();
                break;
            case "sub":
                factory = new SubFactory();
                break;
            case "mul":
                factory = new MulFactory();
                break;
            case "div":
                factory = new DivFactory();
                break;
        }
        operation = factory.createOperation();
        return operation;
    }
}
```

## 簡單工廠 VS 工廠方法

* 簡單工廠模式的最大優點在於工廠類別中包含了必要的邏輯判斷，根據用戶端的選擇條件動態實例化相關的類別，對用戶端來說去除了具體產品的依賴。
* 盡量將長的程式排成切割成小段，再將每一小段封裝起來，減少每段程式之間的耦合，這樣風險就分散了，需要修改或擴充的難度就降低了。


```java

public class Pow extends Operation {

    public double getResult(double numberA, double numberB) {
        return Math.pow(numberA, numberB);
    }
}
public class Log extends Operation {

    public double getResult(double numberA, double numberB) {
        return Math.log(numberA) / Math.log(numberB);
    }
}

public interface IFactory {
    public Operation createOperation();
}

public class FactoryBasic implements IFactory {
    public Operation createOperation(String type) {
        Operation operation = null;
        switch (type) {
            case "add":
                operation = new Add();
                break;
            case "sub":
                operation = new Sub();
                break;
            case "mul":
                operation = new Mul();
                break;
            case "div":
                operation = new Div();
                break;
        }
        return operation;
    }
}

public class FactoryAdvanced implements IFactory {
    public Operation createOperation(String type) {
        Operation operation = null;
        switch (type) {
            case "pow":
                operation = new Pow();
                break;
            case "log":
                operation = new Log();
                break;
        }
        return operation;
    }
}

public class OperationFactory {
    public static Operation createOperation(String type) {
        IFactory factory = null;
        Operation operation = null;
       switch (type) {
            case "add":
            case "sub":
            case "mul":
            case "div":
                factory = new FactoryBasic();
                break;
            case "pow":
            case "log":
                factory = new FactoryAdvanced();
                break;
        }
        operation = factory.createOperation(type);
        return operation;
    }
}
```

* 工廠方法模式 定義一個用於創建物件的介面 讓子類別決定產生實體哪一個類別 工廠方法使一個類別的產生實體延遲到其他子類別

## 簡單工廠 + 策略 + 裝飾 + 工廠方法


```java
public interface IFactory {
    public ISale createSale();
}

public class CashRebateReturnFactory implements IFactory {
    private double moneyRebate = 1d;
    private double moneyReturn = 0d;
    private double moneyCondition = 0d;

    public CashRebateReturnFactory(double moneyRebate, double moneyReturn, double moneyCondition) {
        this.moneyRebate = moneyRebate;
        this.moneyReturn = moneyReturn;
        this.moneyCondition = moneyCondition;
    }
    // 先打X折，再滿M額返N元
    public ISale createSale() {
        CashNormal cashNormal = new CashNormal();
        CashRebate cashRebate = new CashRebate(this.moneyRebate);
        CashReturn cashReturn = new CashReturn(this.moneyReturn, this.moneyCondition);
        cashRebate.decorate(cashNormal); // 滿額返N元
        cashReturn.decorate(cashRebate); // 打X折
        return cashReturn;
    }
}
// 先滿M額返N元，再打X折
public class CashReturnRebateFactory implements IFactory {
    private double moneyRebate = 1d;
    private double moneyReturn = 0d;
    private double moneyCondition = 0d;

    public CashReturnRebateFactory(double moneyRebate, double moneyReturn, double moneyCondition) {
        this.moneyRebate = moneyRebate;
        this.moneyReturn = moneyReturn;
        this.moneyCondition = moneyCondition;
    }
    public ISale createSale() {
        CashNormal cashNormal = new CashNormal();
        CashReturn cashReturn = new CashReturn(this.moneyReturn, this.moneyCondition);
        CashRebate cashRebate = new CashRebate(this.moneyRebate);
        cashReturn.decorate(cashNormal); // 滿額返N元
        cashRebate.decorate(cashReturn); // 打X折
        return cashRebate;
    }
}

public class CashContext {
    private ISale cs;

    public CashContext(int cashType)
    {
        IFactory factory = null;
        switch (cashType) {
            case 1: // 原價
                factory = new CashRebateReturnFactory(1d, 0d, 0d);
                break;
            case 2: // 打8折
                factory = new CashRebateReturnFactory(0.8d, 0d, 0d);
                break;
            case 3: // 打7折
                factory = new CashRebateReturnFactory(0.7d, 0d, 0d);
                break;
            case 4: // 滿300返100
                factory = new CashRebateReturnFactory(1d, 300d, 100d);
                break;
            case 5: // 先打8折，滿500返200
                factory = new CashRebateReturnFactory(0.8d, 500d, 200d);
                break;
            case 6: // 先滿200返50，再打7折
                factory = new CashReturnRebateFactory(0.7d, 200d, 50d);
                break;
        }
        this.cs = factory.createSale();
    }

    public double getResult(double money) {
        return this.cs.getResult(money);
    }
}
```
* 工廠方法模式是簡單工廠模式的進一步抽象和推廣  
* 對於複雜參數的建構物件，可以很好地對外層遮蔽程式的複雜性  
* 很好的解耦能力