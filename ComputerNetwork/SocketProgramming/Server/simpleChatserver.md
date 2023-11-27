## 간단한 chat 서버
```
package server;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.ServerSocket;
import java.net.Socket;
import java.util.ArrayList;

public class SimpleChatServer {
    private ArrayList<PrintWriter> clientOutputStreams;

    private void broadcastMessage(String message) {
        for (PrintWriter clientOutputStream : clientOutputStreams) {
            clientOutputStream.println(message);
        }
    }

    public void go() {
        System.out.println("Running ChatServer");
        clientOutputStreams = new ArrayList<>();
        boolean ENABLE_AUTO_FLUSH = true;

        try (ServerSocket serverSocket = new ServerSocket(5502)) {
            while (true) {
                Socket socket = serverSocket.accept();
                PrintWriter printWriter = new PrintWriter(socket.getOutputStream(), ENABLE_AUTO_FLUSH);
                clientOutputStreams.add(printWriter);

                String remote = socket.getRemoteSocketAddress().toString();
                System.out.println("[ChatServer 🤠] " + remote + " connected");
                new Thread(()-> {
                    try (BufferedReader br = new BufferedReader(new InputStreamReader(socket.getInputStream()))) {
                        br.lines().forEach(message -> {
                            System.out.println("[ChatServer 🤠] " + remote + ": " + message);
                            broadcastMessage(remote + ": " + message);
                        });
                    } catch (IOException ignored) {}
                }).start();
            }
        } catch (IOException e) {
            System.out.println(e.getMessage());
        }
    }
}
```