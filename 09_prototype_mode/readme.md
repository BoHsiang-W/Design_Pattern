## 簡歷程式初步實現

```java
class Resume {
    private String name;
    private String sex;
    private String age;
    private String timeArea;
    private String company;

    public Resume(String name) {
        this.name = name;
    }
    public void setPersonalInfo(String sex, String agey) {
        this.sex = sex;
        this.age = age;
    }
    public void setWorkExperience(String timeArea, String company) {
        this.timeArea = timeArea;
        this.company = company;
    }
    public void display() {
        System.out.println("姓名: " + name);
        System.out.println("性別: " + sex);
        System.out.println("年齡: " + age);
        System.out.println("工作時間: " + timeArea);
        System.out.println("公司: " + company);
    }
}

Resume resume1 = new Resume("張三");
resume1.setPersonalInfo("男", "25");
resume1.setWorkExperience("2015-2018", "ABC公司");

Resume resume2 = new Resume("李四");
resume2.setPersonalInfo("女", "28");
resume2.setWorkExperience("2016-2019", "XYZ公司");

Resume resume3 = new Resume("王五");
resume3.setPersonalInfo("男", "30");
resume3.setWorkExperience("2014-2017", "DEF公司");

resume1.display();
resume2.display();
resume3.display();
```

## 原型模式
* 用原型實例指定創建物件的種類，並且透過拷貝這些原型創建新的物件
* 原型模式其實就是從一個物件來建立另外一個可訂製的物件，而且不須知道任何建立的細節

* Prototype <- client
    * ConcretePrototypeA
    * ConcretePrototypeB

```java
abstract class Prototype implements Cloneable{
    private String id;
    public Prototype(String id) {
        this.id = id;
    }
    public String getId() {
        return id;
    }
    public Object clone() {
        Object obj = null;
        try {
            obj = super.clone();
        } catch (CloneNotSupportedException exception) {
            System.err.println("Clone not supported");
        }
        return obj;
    }
}
class ConcretePrototype extends Prototype {
    public ConcretePrototype(String id) {
        super(id);
    }
}

ConcretePrototype prototype1 = new ConcretePrototype("1");
System.out.println("Prototype ID: " + prototype1.getId());

ConcretePrototype prototype2 = (ConcretePrototype) prototype1.clone();
System.out.println("Cloned Prototype ID: " + prototype2.getId());
```

## 簡歷的原型實現

```java
class Resume implements Cloneable {
    private String name;
    private String sex;
    private String age;
    private String timeArea;
    private String company;

    public Resume(String name) {
        this.name = name;
    }

    public Resume clone() {
        Resume clone = null;
        try {
            clone = (Resume) super.clone();
        } catch (CloneNotSupportedException exception) {
            System.err.println("Clone not supported");
        }
        return clone;
    }
}

Resume resume1 = new Resume("張三");
resume1.setPersonalInfo("男", "25");
resume1.setWorkExperience("2015-2018", "ABC公司");

Resume resume2 = resume1.clone();
resume2.setPersonalInfo("男", "25");
resume2.setWorkExperience("2015-2018", "ABC公司");

Resume resume3 = resume1.clone();
resume3.setPersonalInfo("女", "28");
resume3.setWorkExperience("2016-2019", "XYZ公司");

resume1.display();
resume2.display();
resume3.display();
```

* 一般在初始化的資訊不發生變化的情況下，複製是最好的辦法，既隱藏了物件建立的細節，又對性能有很大的提高
* 不用重新初始化物件，而是動態地獲得物件執行時期的狀態

## 淺複製與深複製
* super.clone() 方法是這樣：如果欄位是數值型態，則對該欄位執行逐位元複製；如果欄位是參考型態，則複製引用但不複製引用的物件，因此原始物件及其副本引用同一物件

```java
class WorkExperience{

    private String timeArea;
    
    public String getTimeArea() {
        return this.timeArea;
    }

    public void setTimeArea(String timeArea) {
        this.timeArea = timeArea;
    }

    private String company;
    public String getCompany() {
        return this.company;
    }
    public void setCompany(String company) {
        this.company = company;
    }
}
class Resume implements Cloneable {
    private String name;
    private String sex;
    private String age;

    private WorkExperience work;
    public Resume(String name) {
        this.name = name;
        this.work = new WorkExperience();
    }
    public void setPersonalInfo(String sex, String age) {
        this.sex = sex;
        this.age = age;
    }

    public void setWorkExperience(String timeArea, String company) {
        this.work.setTimeArea(timeArea);
        this.work.setCompany(company);
    }

    public void display(){
        System.out.println("姓名: " + this.name);
        System.out.println("性別: " + this.sex);
        System.out.println("年齡: " + this.age);
        System.out.println("工作經歷: " + this.work.getTimeArea() + " " + this.work.getCompany());
    }


    public Resume clone() {
        Resume Obj = null;
        try {
            Obj = (Resume) super.clone();
        } catch (CloneNotSupportedException exception) {
            System.err.println("Clone not supported");
        }
        return Obj;
    }
}
Resume resume1 = new Resume("張三");
resume1.setPersonalInfo("男", "25");
resume1.setWorkExperience("2015-2018", "ABC公司");

Resume resume2 = resume1.clone();
resume2.setWorkExperience("2016-2019", "XYZ公司");

Resume resume3 = resume1.clone();
resume3.setPersonalInfo("女", "28");
resume3.setWorkExperience("2014-2017", "DEF公司");

resume1.display();
resume2.display();
resume3.display();
```

* 淺複製:被複製物件的所有變數都含有與原來物件相同的值，而所有的對其他物件的引用都仍然指向原來的物件。
* 深複製:深複製把引用物件的變數指向複製過的新物件，而非原有的被引用的物件


## 簡歷的深複製實現

```java
class WorkExperience{

    private String timeArea;
    
    public String getTimeArea() {
        return this.timeArea;
    }

    public void setTimeArea(String timeArea) {
        this.timeArea = timeArea;
    }

    private String company;
    public String getCompany() {
        return this.company;
    }
    public void setCompany(String company) {
        this.company = company;
    }
    public WorkExperience clone() {
        WorkExperience object = null;
        try {
            object = (WorkExperience) super.clone();
        } catch (CloneNotSupportedException exception) {
            System.err.println("Clone not supported");
        }
        return object;
    }
}

class Resume implements Cloneable {
    private String name;
    private String sex;
    private String age;

    private WorkExperience work;
    public Resume(String name) {
        this.name = name;
        this.work = new WorkExperience();
    }
    public void setPersonalInfo(String sex, String age) {
        this.sex = sex;
        this.age = age;
    }

    public void setWorkExperience(String timeArea, String company) {
        this.work.setTimeArea(timeArea);
        this.work.setCompany(company);
    }

    public void display(){
        System.out.println("姓名: " + this.name);
        System.out.println("性別: " + this.sex);
        System.out.println("年齡: " + this.age);
        System.out.println("工作經歷: " + this.work.getTimeArea() + " " + this.work.getCompany());
    }


    public Resume clone() {
        Resume Obj = null;
        try {
            Obj = (Resume) super.clone();
            Obj.work = this.work.clone(); // 深複製 WorkExperience
        } catch (CloneNotSupportedException exception) {
            System.err.println("Clone not supported");
        }
        return Obj;
    }
}
```

