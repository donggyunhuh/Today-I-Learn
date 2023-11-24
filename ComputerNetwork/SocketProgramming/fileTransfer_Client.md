# 파일을 전송하는 클라이언트

```java
import javax.swing.*;
import java.io.DataOutputStream;
import java.io.File;
import java.io.FileInputStream;
import java.io.IOException;
import java.net.Socket;

public class Main {
    public static void main(String[] args) {
        JFrame frame = new JFrame("Transfer Client");
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setSize(800, 600);

        JButton button = new JButton("Connect & Send");
        button.addActionListener(event -> connectAndSend("117.16.243.99", 5501));
        frame.getContentPane().add(button);

        frame.setVisible(true);
    }

    private static void connectAndSend(String ip, int port){
        try (Socket socket = new Socket(ip, port)) {
            JFileChooser fileChooser = new JFileChooser();
            int state = fileChooser.showOpenDialog(null);
            if (state == JFileChooser.APPROVE_OPTION) {
                String path = fileChooser.getSelectedFile().getPath();
                String filename = fileChooser.getSelectedFile().getName();

                File file = new File(path);
                FileInputStream fis = new FileInputStream(file);

                DataOutputStream dos = new DataOutputStream(socket.getOutputStream());
                dos.writeInt(filename.getBytes().length);
                dos.write(filename.getBytes());
                dos.writeLong(file.length());
                byte[] buffer = new byte[1024];
                int nRead = 0;
                while ((nRead = fis.read(buffer, 0, buffer.length)) != -1) {
                    dos.write(buffer, 0, nRead);
                }
                fis.close();
                dos.close();
            }
        } catch (IOException e) {
            System.out.println(e.getMessage());
        }
    }
}
```