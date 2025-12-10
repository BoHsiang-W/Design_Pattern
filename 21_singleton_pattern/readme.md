## Singleton Pattern

### Form example:
```java
public class Test{
    public static void main(String[] args){
        new SingletonWindow();
    }
}

class SingletonWindow{
    public SingletonWindow(){
        JFrame frame = new JFrame("Singleton Pattern Example");
        frame.setSize(1024, 768);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        
        Jpanel panel = new JPanel();
        frame.add(panel);
        panel.setLayout(null);

        JButton button = new JButton("Open Singleton Window");
        button.setBounds(10, 10, 120, 25);
        button.addActionListener(new ActionListener(){
            public void actionPerformed(ActionEvent e){
                JFrame toolkit = new JFrame("Singleton Window");
                toolkit.setSize(400, 300);
                toolkit.setLocation(200, 200);
                toolkit.setResizable(false);
                toolkit.setVisible(true);
                toolkit.setDefaultCloseOperation(JFrame.DISPOSE_ON_CLOSE);
                toolkit.setVisible(true);
            }
        });
        panel.add(button);
        frame.setVisible(true);
    }
}
```

### Check if the object is null
```java

class SingletonWindow{
    public SingletonWindow(){
        JFrame frame = new JFrame("Singleton Pattern Example");
        frame.setSize(1024, 768);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        
        Jpanel panel = new JPanel();
        frame.add(panel);
        panel.setLayout(null);

        JButton button = new JButton("Open Singleton Window");
        button.setBounds(10, 10, 120, 25);
        button.addActionListener(new ActionListener(){
            JFrame toolkit;
            public void actionPerformed(ActionEvent e){
                if (toolkit == null || !toolkit.isVisible()) {
                    toolkit.setSize(400, 300);
                    toolkit.setLocation(200, 200);
                    toolkit.setResizable(false);
                    toolkit.setVisible(true);
                    toolkit.setDefaultCloseOperation(JFrame.DISPOSE_ON_CLOSE);
                    toolkit.setVisible(true);
                }
            }
        });
        panel.add(button);
        frame.setVisible(true);
    }
}
```

```java

class SingletonWindow{
    public SingletonWindow(){
        JFrame frame = new JFrame("Singleton Pattern Example");
        frame.setSize(1024, 768);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        
        Jpanel panel = new JPanel();
        frame.add(panel);
        panel.setLayout(null);

        JButton button = new JButton("Open Singleton Window");
        button.setBounds(10, 10, 120, 25);
        button.addActionListener(new ToolkitActionListener());
        panel.add(button);

        JButton button2 = new JButton("Open Singleton Window");
        button2.setBounds(10, 40, 120, 25);
        button2.addActionListener(new ToolkitActionListener());
        panel.add(button2);

    }
}

class ToolkitActionListener implements ActionListener{
    private static JFrame toolkit;

    public void actionPerformed(ActionEvent e){
        if (toolkit == null || !toolkit.isVisible()) {
            toolkit = new JFrame("Singleton Window");
            toolkit.setSize(400, 300);
            toolkit.setLocation(200, 200);
            toolkit.setResizable(false);
            toolkit.setVisible(true);
            toolkit.setDefaultCloseOperation(JFrame.DISPOSE_ON_CLOSE);
            toolkit.setVisible(true);
        }
    }
}
```

```java
class ToolkitListener implements ActionListener{
    private static JFrame toolkit;

    public void actionPerformed(ActionEvent e){
        if (toolkit == null || !toolkit.isVisible()) {
            toolkit = new JFrame("Singleton Window");
            toolkit.setSize(400, 300);
            toolkit.setLocation(200, 200);
            toolkit.setResizable(false);
            toolkit.setVisible(true);
            toolkit.setDefaultCloseOperation(JFrame.DISPOSE_ON_CLOSE);
            toolkit.setVisible(true);
        }
    }
}
```
所有的類別都有建構方法，不編碼則系統預設生成空的建構方法，若有顯示定義的建構方法，預設的建構方法就會故障。

```java
class Toolkit extends JFrame{
    private static Toolkit toolkit;

    private Toolkit(String title){
        super(title);
    }

    public static Toolkit getInstance(){
        if(toolkit == null || !toolkit.isVisible()){
            toolkit = new Toolkit("Singleton Window");
            toolkit.setSize(400, 300);
            toolkit.setLocation(200, 200);
            toolkit.setResizable(false);
            toolkit.setDefaultCloseOperation(JFrame.DISPOSE_ON_CLOSE);
        }
        return toolkit;
    }
}

class ToolkitActionListener implements ActionListener{
    public void actionPerformed(ActionEvent e){
        // Toolkit toolkit = Toolkit.getInstance();
        toolkit.setVisible(true);
    }
}
```

## 單例模式
單例模式(Singleton)，保證一個類別僅有一個實例，並提供一個存取它的全域存取點。

通常我們可以讓一個全域變數使得一個物件被存取，但它不能防止你實例化多個物件。一個最好的辦法就是，讓類別自身負責儲存它的唯一實例。這個類別可以保證沒有其他實例可以被建立，並且它可以提供一個存取該實例的方法。

```java
class Singleton{
    private static Singleton instance;

    private Singleton(){}

    public static Singleton getInstance(){
        if(instance == null){
            instance = new Singleton();
        }
        return instance;
    }
}

// client code
public class Test{
    public static void main(String[] args){
        Singleton singleton1 = Singleton.getInstance();
        Singleton singleton2 = Singleton.getInstance();

        if(singleton1 == singleton2){
            System.out.println("Both references point to the same instance.");
        } else {
            System.out.println("Different instances exist.");
        }
    }
}
```
單例模式因為Singleton類別封裝它的唯一實例，這樣它可以嚴格地控制客戶怎樣存取它以及何時存取它，簡單地說就是對唯一實例的受控存取。

### 多執行緒時的單例

```java
class Singleton{
    private static Singleton instance;

    private Singleton(){}

    public static synchronized Singleton getInstance(){
        if(instance == null){
            instance = new Singleton();
        }
        return instance;
    }
}
```

### 雙重鎖定
```java
class Singleton{
    private static volatile Singleton instance;

    private Singleton(){}

    public static Singleton getInstance(){
        if(instance == null){
            synchronized(Singleton.class){
                if(instance == null){
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```
### 靜態初始化

```java
class Singleton{
    private static Singleton instance = new Singleton();

    private Singleton(){}

    public static Singleton getInstance(){
        return instance;
    }
}
```
這種靜態初始化的方式是在自己被載入時就將自己實例化，所以被形象地稱之為餓漢式單例類別，原先的單例模式處理方式，是要在第一次被引用時，才會將自己實例化，所以就被稱為懶漢式單例類別。