# 간단한 오브젝트 전송 GUI

## Main
```java
import javax.swing.*;
import java.awt.*;
import java.io.IOException;
import java.io.ObjectOutputStream;
import java.io.Serializable;
import java.net.Socket;
import server.SimpleObjectServer;

public class Main {

    public static void main(String[] args) {

        JFrame frame = new JFrame();
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setSize(500,200);
        frame.setLocationRelativeTo(null);

        JPanel panel = new JPanel(new GridLayout(0,2));
        JTextField x = new JTextField();
        JTextField y = new JTextField();
        panel.add(x);
        panel.add(y);

        Container container = frame.getContentPane();
        container.add(BorderLayout.CENTER,panel);

        JButton button = new JButton("SEND");

        container.add(button, BorderLayout.SOUTH);

        try {
            Socket s = new Socket("117.16.243.99", 5503);
            ObjectOutputStream oos = new ObjectOutputStream(s.getOutputStream());
            button.addActionListener((e)->send(oos,x.getText(),y.getText()));


        }catch (IOException e){
            e.printStackTrace();
        }

        frame.setVisible(true);

    }
    private static void send(ObjectOutputStream oos, String x, String y){
        try {
            SimpleObjectServer.DrawPoint dp = new SimpleObjectServer.DrawPoint(Integer.parseInt(x), Integer.parseInt(y));
            oos.writeObject(dp);
        }catch(IOException e){
            e.printStackTrace();
        }

    }
}


```

server Package

```java
package server;

import java.io.Serial;
import java.io.Serializable;
public class SimpleObjectServer {
    public static class DrawPoint implements Serializable{
        @Serial
        private static final long serialVersionUID = 123412341234L;
        private final int x;
        private final int y;

        public DrawPoint(int x, int y){
            this.x = x;
            this.y = y;
        }
    }
}

```
