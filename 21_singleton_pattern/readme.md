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
