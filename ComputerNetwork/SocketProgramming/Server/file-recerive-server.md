# 파일을 수신하는 서버

```java
package server;

import java.io.DataInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.net.ServerSocket;
import java.net.Socket;
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;

public class SimpleTransferServer {
    public void go() {
        try (ServerSocket serverSocket = new ServerSocket(5501)) {
            System.out.println("Running TransferServer");
            while (true) {
                Socket socket = serverSocket.accept();
                new Thread(new ClientHandler(socket)).start();
            }
        } catch (IOException e) {
            System.out.println(e.getMessage());
        }
    }

    private void mv(String filename) throws IOException {
        new ProcessBuilder("mv", filename, "/Users/hxxn85/Desktop/"+filename).start();
    }

    private class ClientHandler implements Runnable {
        private Socket socket;
        DateTimeFormatter dateTimeFormatter;

        public ClientHandler(Socket socket) {
            this.socket = socket;
            dateTimeFormatter = DateTimeFormatter.ofPattern("MM/dd HH:mm:ss");
        }

        public void run() {
            try {
                String remote = socket.getRemoteSocketAddress().toString();
                DataInputStream dis = new DataInputStream(socket.getInputStream());

                int nFileNameBytes = dis.readInt();
                byte[] bytes = new byte[nFileNameBytes];
                dis.read(bytes, 0, nFileNameBytes);
                String filename = new String(bytes);

                FileOutputStream fos = new FileOutputStream(filename);
                Long x = dis.readLong();
                byte[] buffer = new byte[1024];
                int nRead;
                while ((nRead = dis.read(buffer, 0, buffer.length)) != -1) {
                    fos.write(buffer, 0, nRead);
                    fos.flush();
                }
                fos.close();
                dis.close();

                String now = LocalDateTime.now().format(dateTimeFormatter);
                System.out.println("[TransferServer 😜] (" + now + ") " + remote + ": " + filename);
                mv(filename);
            } catch (IOException ignored) {}
        }
    }
}

```