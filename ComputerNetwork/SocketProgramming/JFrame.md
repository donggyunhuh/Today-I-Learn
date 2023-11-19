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
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.io.*;
import java.net.Socket;
import java.util.Random;

public class Main {
static Socket socket;

    public static void main(String[] args) {

        JFrame frame = new JFrame("My Frame");
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setSize(400, 300);


        Container container = frame.getContentPane();
        container.setLayout(new BorderLayout());


        JButton button1 = new JButton("Connect");
        button1.addActionListener(e -> {
            try{
                socket = new Socket("117.16.243.99",5501);
                System.out.println("연결!");
            } catch (IOException ex){
                throw new RuntimeException(ex);
            }
        });


        JButton button2 = new JButton("Send");
        button2.addActionListener((e -> {
            JFileChooser jFileChooser =new JFileChooser();
            int ret = jFileChooser.showOpenDialog(null);
            if(ret == JFileChooser.APPROVE_OPTION){
                try {
                    DataOutputStream dos = new DataOutputStream(socket.getOutputStream());
                    String path = jFileChooser.getSelectedFile().getPath();
                    String name = jFileChooser.getSelectedFile().getName();
                    //4bytes -> 파일명 크기
                    dos.writeInt(name.getBytes().length);
                    dos.write(name.getBytes());
                    //8 Byte -> 파일 크기

                    File file = new File(path);
                    long size = file.length();
                    dos.writeLong(size);


                    // 파일전솓
                    FileInputStream fis = new FileInputStream(path);
                    int nRead = 0;
                    byte[] buffer = new byte[1024];
                    while((nRead = fis.read(buffer,0,buffer.length)) != -1){
                        dos.write(buffer,0,nRead);
                    }
                } catch (IOException ignored){

                }
            }
        }));

        frame.setVisible(true);
    }
}
```