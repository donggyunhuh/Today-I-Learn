# Flow Control

- 윈도우 사이즈를 결정하기 위한 기능
- 윈도우 상황이 좋으면 윈도우 사이즈를 최대한 키우기 

-> 윈도우 사이즈를 결정할 때 네트워크 상홍을 고려해야 하고, 수신단이 얼만큼의 속도로 데이터를 받을 준비가 되어있을지 고려해야합니다.(수신단의 버퍼 상태)

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F1Q6Cf%2Fbtrhy2Vpj6e%2F0kqZoTrQ8W7BiuBfOcHK20%2Fimg.png">

UDP보다 많은 정보를 요구하는 것을 볼 수 있습니다. TCP는 길이가 길다보니 오버헤드가 많습니다.

대표적으로 소스포트/데스티네이션 포드가 들어있습니다. UDP와 같죠.

시퀀스넘버가 필드에 들어있습니다. (32비트 차지)

액넘버도 사용됩니다(32비트)

리시브 윈도우 : 플로우 컨트롤에서 배웁니다.

옵션필드 : 미래에 추가될 정보가 있으니 남겨두는 필드입니다. TCP는 갈아엎기 힘들기 때문에 추가 옵션을 통해 많은 기능을 제공하고자 한다.

U(urgent): 긴급한 패킷 표시하기 위한 용도입니다. (1 or 0) 실제로는 잘 안씁니다.

A(acknowledgement):이 패킷이 acknowledgement을 위한 것인지 데이터인지를 구분하기 위한 식별자입니다.

P(push data)

R,S,F : 연결에 필요한 필드, 연결을 맺고 끊는 부분 설명할 때 알 수 있을 예정


##  TCP에서 시퀀스넘버와 ACKs

시퀀스 넘버를 붙일 때 각 TCP 세그먼트(바이트 넘버)의 1번째 바이트 넘버가 시퀀스 넘버가 됩니다. 1000바이트 단위의 MSS가 있다고 하면 1번째 세그먼트는 0이 시퀀스 넘버이고, 2번째 세그먼트는 1000이 시퀀스 넘버입니다.

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbdOWjD%2FbtrhzRFJFA4%2FVfWPYi9hp1Y1OYTf4NT4D0%2Fimg.png">

- CK넘버는 첫번째 세그먼트를 받았을 때 그 다음의 시퀀스 번호를 ACK (ex 1000, 2000)으로 보
- TCP는 cummlative ack 방식
