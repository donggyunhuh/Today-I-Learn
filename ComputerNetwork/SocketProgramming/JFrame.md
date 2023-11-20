# Java 소켓 통신 예제 설명

이 코드는 Java를 사용하여 소켓 통신을 구현하는 간단한 예제입니다. 이 예제는 클라이언트 애플리케이션을 구현하며, 사용자는 "Connect" 버튼을 통해 서버에 연결하고 "Send" 버튼을 통해 파일을 전송할 수 있습니다.

## GUI 설정

먼저, Swing 라이브러리를 사용하여 GUI를 설정합니다. JFrame을 생성하고 컨테이너를 설정한 다음, "Connect" 및 "Send" 버튼을 추가합니다.

## Connect 버튼 동작

"Connect" 버튼을 클릭하면 서버에 소켓 연결을 시도합니다


## Send 버튼 동작

"Send" 버튼을 클릭하면 파일을 선택하고 서버에 전송합니다. JFileChooser를 사용하여 파일 선택 대화상자를 열고 선택한 파일의 이름과 크기를 서버로 전송한 후 파일 내용을 전송합니다.

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