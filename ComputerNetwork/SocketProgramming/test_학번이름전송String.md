# 학번이름 전송 소켓

```java
import javax.swing.*;
import java.awt.*;
import java.io.*;
import java.net.Socket;

public class Main {
    public static void main(String[] args) {
        JFrame frame = new JFrame();
        frame.setSize(400, 200);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setLocationRelativeTo(null);

        JPanel panel = new JPanel(new GridLayout(0, 1));
        JTextField id = new JTextField();

        panel.add(id);

        Container container = frame.getContentPane();
        container.add(BorderLayout.CENTER, panel);

        JButton button = new JButton("send");
        container.add(button, BorderLayout.SOUTH);

        try {
            Socket socket = new Socket("117.16.243.99", 5507);
            ObjectOutputStream oos = new ObjectOutputStream(socket.getOutputStream());
            button.addActionListener(event -> send(oos, id.getText()));
            ObjectInputStream ois = new ObjectInputStream(socket.getInputStream());
            button.addActionListener(event -> receive(ois));


        } catch (IOException e) {
            e.printStackTrace();

        }
        frame.setVisible(true);

        }
       public static void send(ObjectOutputStream oos, String id) {
        try{ oos.writeObject(id);
            System.out.println("학번 : " + id );

        }catch(IOException e){
            e.printStackTrace();

        }

    }

    public static void receive(ObjectInputStream ois){
        try {
            int x = ois.readInt();

            System.out.println(x);

        } catch (IOException e) {
            e.printStackTrace();

        }

    }
    
}



```