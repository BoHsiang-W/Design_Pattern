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

