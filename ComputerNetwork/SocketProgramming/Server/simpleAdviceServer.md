## 간단한 말 어구를 리턴하는 서버
```java
package server;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.ServerSocket;
import java.net.Socket;
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;

public class SimpleAdviceServer {
    public void go() {
        LocalDateTime localDateTime;
        DateTimeFormatter formatter = DateTimeFormatter.ofPattern("MM/dd HH:mm:ss");
        try (ServerSocket serverSocket = new ServerSocket(5500)) {
            System.out.println("Running AdviceServer");
            while (true) {
                Socket socket = serverSocket.accept();
                String remote = socket.getRemoteSocketAddress().toString();
                PrintWriter printWriter = new PrintWriter(socket.getOutputStream());
                String advice = getAdvice();
                printWriter.println(advice);
                printWriter.close();
                localDateTime = LocalDateTime.now();
                String now = localDateTime.format(formatter);
                System.out.println("[AdviceServer 😜] (" + now + ") " + remote + ": " + advice);
            }
        } catch (IOException e) {
            System.out.println(e.getMessage());
        }
    }

    private String getAdvice() throws IOException {
        Process fortune = Runtime.getRuntime().exec("fortune");
        BufferedReader bufferedReader = new BufferedReader(new
                InputStreamReader(fortune.getInputStream()));

        return bufferedReader.readLine();
    }
}

```