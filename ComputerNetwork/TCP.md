# TCP 

## 3-Way Handshake

- TCP 통신을 이용하여 데이터를 전송하기 위해 네트워크 연결을 설정(Connection Establish) 하는 과정

- 양쪽 모두 데이터를 전송할 준비가 되었다는 것을 보장하고, 실제로 데이터 전달이 시작하기 전에 한 쪽이 다른 쪽이 준비되었다는 것을 알 수 있도록 한다.
- TCP/IP 프로토콜을 이용해서 통신을 하는 응용 프로그램이 데이터를 전송하기 전에 먼저 정확한 전송을 보장하기 위해 상대방 컴퓨터와 사전에 세션을 수립하는 과정을 의미

## 기본매커니즘
- TCP 통신은 PAR (Positive Acknowledgement with Re-transmission) 을 통해 신뢰적인 통신을 제공

#### PAR을 사용하는 기기는 ack을 받을 때까지 데이터 유닛을 재전송

수신자가 데이터 유닛(세그먼트)이 손상된것을 확인하면(Error Detection에 사용되는 transport layer의 checksum을 활용), 해당 세그먼트를 없앤다. 그러면 sender는 positive ack이 오지 않은 데이터 유닛을 다시 보내야한다.

⇒ 이 과정에서 클라이언트와 서버 사이에서 3개의 Segment가 교환되는 것을 확인할 수 있다. 이것이 바로 3-way handshake의 기본 매커니즘


### 상세 과정 1,2,3 순서
1. 클라이언트 : SYNbit = 1,Seq = x
2. 서버 : SYNbit = 1, Seq = y, ACKbit=1, ACKnum = x+1
3. 클라이언트 : ACKbit = 1, ACKnum = y+1

### 왜 3번 핸드셰이크를 하나?
- 경우 1 : 연결을 2번만 수행할 경우 (1,2) -> 서버측은  연결을 완료하고 클라이언트의 요청을 기다리는데, 클라이언트가 연결을 끊어버리면 서버 자원은 낭비된다.
- 경우 2 : 2번에서, 서버가 전송한 패킷이 loss된다면, 클라이언트는 계속 연결요청을 다시 보낼 것
- 경우 3 : 1번에서, 서버가 현재 작동하지 않는데 패킷요청을 계속 한다면 클라이언트의 자원 낭비



# TCP : Closing Connection
1. 클라이언트 : FINbit = 1, seq = x 
2. 서버 : ACKbit = 1; ACKnum = x+1 -> wait for server close
3. 서버 : FINbit = 1, seq = y
4. 클라이언트 : ACKbit = 1, ACKnum = y+1

## 설명
연결 끊는 것을 서버에게 허락받고, 서버에서도 연결 끊겠다는 것을 기다리고 양쪽 허용을 받으면 state정보를 날려버리고 끊습니다.

위처럼 각각 날릴 수도 있고 piggybacked 도 가능하다. 