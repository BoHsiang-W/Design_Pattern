## 簡單工廠實現 - 收銀軟體
```java
public abstract class CashSuper {
    public abstract double acceptCash(double money, int num);
}
public class CashNormal extends CashSuper {
 
    public double acceptCash(double money, int num) {
        return money * num;
    }
}
public class CashRebate extends CashSuper {
    private double rebate = 1d;
 
    public CashRebate(double rebate) {
        this.rebate = rebate;
    }
 
    public double acceptCash(double money, int num) {
        return money * num * rebate;
    }
}

public class CashReturn extends CashSuper {
    private double moneyCondition = 0d;
    private double moneyReturn = 0d;

    // example: if moneyCondition is 300 and moneyReturn is 100, then if the total amount is 300 or more, it will return 100.
    public CashReturn(double moneyCondition, double moneyReturn) {
        this.moneyCondition = moneyCondition;
        this.moneyReturn = moneyReturn;
    }
 
    public double acceptCash(double money, int num) {
        double total = money * num;
        if (moneyCondition > 0 && total >= moneyCondition) {
            return total - Math.floor(total / moneyCondition) * moneyReturn;
        }
        return total;
    }
}

public class CashFactory {
    public static CashSuper createCashAccept(int type) {
        CashSuper cs = null;
        switch (type) {
            case 1:
                cs = new CashNormal();
                break;
            case 2:
                cs = new CashRebate(0.8d); // 20% discount
                break;
            case 3:
                cs = new CashRebate(0.7d); // 30% discount
                break;
            case 4:
                cs = new CashReturn(300d, 100d); // if total >= 300, return 100
                break;
        }
        return cs;
    }
}
```

## 策略模式 - 收銀軟體
```java
public class CashContext {
    private CashSuper cs;

    public CashContext(CashSuper cs) {
        this.cs = csuper;
    }

    public double getResult(double money, int num) {
        return this.cs.acceptCash(money, num);
    }
}
CashContext cc = null;
switch (type) {
    case 1:
        cc = new CashContext(new CashNormal());
        break;
    case 2:
        cc = new CashContext(new CashRebate(0.8d)); // 20% discount
        break;
    case 3:
        cc = new CashContext(new CashRebate(0.7d)); // 30% discount
        break;
    case 4:
        cc = new CashContext(new CashReturn(300d, 100d)); // if total >= 300, return 100
        break;
}
totalprice = cc.getResult(money, num);
total = total + totalprice;
```
## 策略模式 + 簡單工廠模式
```java
public class CashContext {
    private CashSuper cs;

    public CashContext(int type) {
        switch (type) {
            case 1:
                cs = new CashNormal();
                break;
            case 2:
                cs = new CashRebate(0.8d); // 20% discount
                break;
            case 3:
                cs = new CashRebate(0.7d); // 30% discount
                break;
            case 4:
                cs = new CashReturn(300d, 100d); // if total >= 300, return 100
                break;
        }
    }
    public double getResult(double money, int num) {
        return this.cs.acceptCash(money, num);
    }
}

CashContext cc = new CashContext(discount);
totalprice = cc.getResult(money, num);
total = total + totalprice;

// simple factory 
CashSuper cs = CashFactory.createCashAccept(discount);
totalprice = cs.acceptCash(money, num);

// strategy pattern + simple factory pattern
CashContext cc = new CashContext(discount);
totalprice = cc.getResult(money, num);
```
* 簡單工廠模式須認識 CashSuper,CashFactory.
* 策略模式只須認識 CashContext.