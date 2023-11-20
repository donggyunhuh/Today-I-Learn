# Swing 라이브러리를 사용하여 GUI를 생성하고, 소켓을 통해 서버와 통신하여 채팅 메시지를 주고받는 간단한 채팅 클라이언트를 구현한 예제

``` java
import javax.swing.*; // Swing 라이브러리를 포함시킵니다. GUI 컴포넌트를 만드는데 사용됩니다.
import java.awt.*; // AWT 라이브러리를 포함시킵니다. 윈도우, 패널 등을 만드는데 사용됩니다.
import java.io.*; // 입출력 관련 클래스를 포함시킵니다. BufferedReader, PrintWriter 등이 여기에 속합니다.
import java.net.*; // 네트워크 프로그래밍을 위한 클래스를 포함시킵니다. 여기서는 Socket을 사용합니다.

public class Main {
public static void main(String[] args) throws IOException {
// 메인 윈도우를 생성합니다.
JFrame frame = new JFrame();

        // 윈도우의 크기를 400x300 픽셀로 설정합니다.
        frame.setSize(400,300);
        // 윈도우를 화면 중앙에 배치합니다.
        frame.setLocationRelativeTo(null);
        // 윈도우 닫기 버튼을 누르면 프로그램이 종료되도록 설정합니다.
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

        // 윈도우의 컨텐츠를 담을 컨테이너를 가져옵니다.
        Container container = frame.getContentPane();
        // 채팅 내역을 표시할 텍스트 영역을 생성합니다.
        JTextArea chatArea = new JTextArea();
        // 자동 줄바꿈, 스크롤, 편집 불가 설정 등을 합니다.
        chatArea.setLineWrap(true);
        chatArea.setEnabled(false);
        chatArea.setAutoscrolls(true);
        // 채팅 영역에 스크롤 바를 추가합니다.
        JScrollPane chatPane = new JScrollPane(chatArea);

        // 사용자가 메시지를 입력할 텍스트 필드를 생성합니다.
        JTextField messageField = new JTextField();
        // 메시지를 보낼 버튼을 생성합니다.
        JButton sendButton = new JButton("SEND");
        // 텍스트 필드와 버튼을 담을 패널을 생성합니다.
        JPanel actionPanel = new JPanel(new BorderLayout());
        // 텍스트 필드와 버튼을 패널에 추가합니다.
        actionPanel.add(messageField, BorderLayout.CENTER);
        actionPanel.add(sendButton,BorderLayout.EAST);

        // 채팅 영역과 액션 패널을 윈도우에 추가합니다.
        container.add(chatPane, BorderLayout.CENTER);
        container.add(actionPanel, BorderLayout.SOUTH);

        // 서버에 연결할 소켓을 생성합니다. 여기서는 교수님의 IP 서버 주소와 포트를 사용합니다.
        Socket s = new Socket("117.16.243.99", 5502);
        // 서버로 메시지를 보낼 PrintWriter를 생성합니다.
        PrintWriter pw = new PrintWriter(s.getOutputStream(), true);
        // 메시지 필드와 버튼에 액션 리스너를 추가합니다.
        messageField.addActionListener((e)-> {sendButton(pw, messageField);});
        sendButton.addActionListener(e ->{sendButton(pw, messageField);});

        // 서버로부터 메시지를 받을 BufferedReader를 생성합니다.
        BufferedReader br = new BufferedReader(new InputStreamReader((s.getInputStream())));
        // 서버로부터 오는 메시지를 계속 읽고 화면에 표시하는 스레드를 생성하고 시작합니다.
        Thread receiveTread = new Thread(() -> {
            br.lines().forEach((message) ->{
               chatArea.append(message + '\n');
               chatArea.setCaretPosition(chatArea.getDocument().getLength());
            });
        });
        receiveTread.start();

        // 모든 설정이 완료되면 윈도우를 화면에 표시합니다.
        frame.setVisible(true);
    }

    // 사용자가 메시지를 보내는 기능을 하는 메소드입니다.
    private static void sendButton(PrintWriter pw, JTextField messageField) {
        pw.println(messageField.getText()); // 메시지 필드의 내용을 서버로 보냅니다.
        messageField.setText(""); // 메시지 필드를 비웁니다.
        messageField.requestFocus(); // 메시지 필드에 다시 포커스를 맞춥니다.
    }
}
```