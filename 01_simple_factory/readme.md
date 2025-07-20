## 業務的封裝
```java
public class Operation {
    public static double getResult(double numA, double numB, String operate) {
        double result = 0d;
        switch (operate){
            case "+":
                result = numA + numB;
                break;
            case "-":
                result = numA - numB;
                break;
            case "*":
                result = numA * numB;
                break;
            case "/":
                result = numA / numB;
                break;
            case "sqrt":
                result = Math.sqrt(numA);
                break;
        }
        return result;
    }
}

Scanner scanner = new Scanner(System.in);
System.out.println("請輸入第一個數字:");
double numA = Double.parseDouble(scanner.nextLine());
System.out.println("請輸入運算符號 (+, -, *, /, sqrt):");
String operate = scanner.nextLine();
System.out.println("請輸入第二個數字:");
double numB = Double.parseDouble(scanner.nextLine());

double result = Operation.getResult(numA, numB, operate);
System.out.println("計算結果: " + result);
```

## 簡單工廠模式
```java

public class Add extends Operation {
    public double getResult(double numA, double numB) {
        return numA + numB;
    }
}
public class Sub extends Operation {
    public double getResult(double numA, double numB) {
        return numA - numB;
    }
}
public class Mul extends Operation {
    public double getResult(double numA, double numB) {
        return numA * numB;
    }
}
public class Div extends Operation {
    public double getResult(double numA, double numB) {
        if (numB == 0) {
            System.out.println("除數不能為零");
            throw new ArithmeticException("除數不能為零");
        }
        return numA / numB;
    }
}

public class OperationFactory {
    public static Operation createOperation(String operate) {
        Operation operation = null;
        switch (operate) {
            case "+":
                operation = new Add(): 
                break;
            case "-":
                operation = new Sub();
                break;
            case "*":
                operation = new Mul();
                break;
            case "/":
                operation = new Div();
                break;
        }
        return operation;
    }
}

Operation operation = OperationFactory.createOperation("+");
double result = operation.getResult(5, 3);
System.out.println("計算結果: " + result);