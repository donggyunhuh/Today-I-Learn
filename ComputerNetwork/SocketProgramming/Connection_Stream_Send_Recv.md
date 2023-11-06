# 소켓 프로그래밍

## 정의
- 네트워크상에서 서로 다른 시스템 간의 통신을 가능하게 하는 프로그래밍 방식

- 소켓은 네트워크 통신의 끝점(endpoint)을 추상화한 것으로, 데이터를 송수신하는데 사용

- TCP, UDP 사용

### 주요 구성 요소

- 소켓(Socket) : 네트워크 상에서 프로그램간 통신을 위한 인터페이스. IP주소와 포트 번호의 조합을 사용하여 특정 서비스에 대한 엔드포인트 역할
- 클라이언트(Client) : 서비스를 요구하는 주체, 서버에 연결을 요청하고 데이터를 송수신
- 서버(Server) : 클라이언트의 요청을 받아들이고 처리한 후 그 결과를 클라이언트에게 보냄
- 포트(Port) : 컴퓨터에서 실행중인 특정 프로그램을 구별하는 논리적인 접속 장소 (IP address - 컴퓨터 식별, Port - 컴퓨터 내에서 실행 중인 특정 프로그램 식별)


### 흐름

1. **서버소켓 생성** : 서버는 listen(), accept() 등의 함수를 이용하여 소켓 생성 - 다음 실행 block, 다음 코드 실행 되지 않음
2. **클라이언트 소켓 생성** : 클라이언트는 connet() 함수 사용
3. **데이터 교환** : send(), recv(), write(), read() 함수 등 사용
4. **소켓 종료** : close() 함수 호출하여 소켓 연결 종료


## 2023-11-06 Computer NetWork Lecture

### 예제의 흐름
1. 소켓 연결
2. InputStream 가져오기
3. InputStream 문자로 읽어오기
4. BuffereReader에서 InputStream문자를 문자열을 읽어오기
5. 콘솔에 문자열 출력하기


#### 소켓 연결하기

**포트번호와 IP 주소로 소켓 연결 설정하기**

```java
Socket socket = new Socket("IP address", PORT Number)
```

**소켓으로부터 입력 스트림 가져오기**
```java
InputSteam inputStream = socket.getInputStream();
```

**가져온 inputStream을 문자로 변환**
```java
InputStreamReader reader = new InputStreamReader(inputStream);
```

**변환한 문자를 문자열로 변환**
```java
BufferedReader bufferedReader = new BufferedReader(reader);
```

##### 위 코드를 한줄로 간결히 정리

```java
BufferdReader bufferedReader = new BufferedReader(InputStreamReader(socket.getInputStream()));
```
## 예제 문제

LAB 01. SIMPLE ADVICE CLIENT
- Connect to the server @ IP address 117.16.243.99 on port 5500
- Once the connection is established, the server sends a random message
- You should print the message to the console
 
```java
import java.io.*;
import java.net.Socket;

public class Main {
    public static void main(String[] args) {
        try {
            Socket socket = new Socket("117.16.243.99", 5500);
            InputStream inputStream = socket.getInputStream(); // 입력 큐
            // 비트열로 읽음

           /*
            InputStreamReader reader =
                 new InputStreamReader(inputStream); // 바이트 -> 문자
           BufferedReader readerReader =
                    new BufferedReader(reader); // 문자 -> 문자열   */

            //위 코드를 한줄로
            BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(socket.getInputStream()));

            String recv = bufferedReader.readLine();
            System.out.println(recv);

        } catch (IOException e){
            System.out.println("에러가 남/n");
        };
    }
}
```