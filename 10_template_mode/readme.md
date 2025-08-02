## 重複=易錯+難改
* TestPaperA
    * +testQuestion1
    * +testQuestion2
    * +testQuestion3
* TestPaperB
    * +testQuestion1
    * +testQuestion2
    * +testQuestion3

```java
class TestPaperA {
    public void testQuestion1() {
        System.out.println("TestPaperA: Question 1");
        System.out.println("Answer: A");
        
    }
    public void testQuestion2() {
        System.out.println("TestPaperA: Question 2");
        System.out.println("Answer: B");
    }
    public void testQuestion3() {
        System.out.println("TestPaperA: Question 3");
        System.out.println("Answer: C");
    }
}
class TestPaperB {
    public void testQuestion1() {
        System.out.println("TestPaperB: Question 1");
        System.out.println("Answer: B");
    }
    public void testQuestion2() {
        System.out.println("TestPaperB: Question 2");
        System.out.println("Answer: A");
    }
    public void testQuestion3() {
        System.out.println("TestPaperB: Question 3");
        System.out.println("Answer: C");
    }
}
```
## 提煉程式
* TestPaper
    * +testQuestion1
    * +testQuestion2
    * +testQuestion3
    * +answer1
    * +answer2
    * +answer3
    * TestPaperA
        * answer1
        * answer2
        * answer3
    * TestPaperB
        * answer1
        * answer2
        * answer3 
```java
abstract class TestPaper {
    public void testQuestion1() {
        System.out.println("Question 1");
        System.out.println("Answer: "+ this.answer1());
    }
    protected abstract String answer1(); // rewrite this method in subclasses
    
    public void testQuestion2() {
        System.out.println("Question 2");
        System.out.println("Answer: "+ this.answer2());
    }
    protected abstract String answer2(); // rewrite this method in subclasses
    
    public void testQuestion3() {
        System.out.println("Question 3");
        System.out.println("Answer: "+ this.answer3());
    }
    protected abstract String answer3(); // rewrite this method in subclasses
}
class TestPaperA extends TestPaper {
    protected String answer1() {
        return "A";
    }

    protected String answer2() {
        return "B";
    }

    protected String answer3() {
        return "C";
    }

}
class TestPaperB extends TestPaper {
    protected String answer1() {
        return "B";
    }

    protected String answer2() {
        return "A";
    }

    protected String answer3() {
        return "C";
    }
}
```

## 範本方法模式特點
* 範本方法模式是透過把不變行為搬移到超類別，去除子類別的重複程式，表現它的優勢
* 當不變的和可變的行為在方法的子類別實現中混合在一起的時候，不變的行為就會在子類別中重複出現
* 透過範本方法模式把這些行為搬移到單一的地方，這樣就幫助子類別擺脫重複的不變行為的糾纏
