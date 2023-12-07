# Fast-Retransmit

- 개요 : 중복된(duplicate) ack을 연속해서 받으면 타임아웃되기 전에 재전송

- 약 3개의 중복 ack을 받으면 재전송합니다. (3 duplicate ack)

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Flfywr%2Fbtrh3oR0dET%2FodWOTzDCxaf9NuUGOPsAK1%2Fimg.png">

- 3 duplicate ack이 있다면 ack을 안받은 것 중 가장 작은 시퀀스 넘버 패킷을 재전송
- 왜? : 중간에 로스가 있어도 뒤의 Ack는 제대로 도착할 가능성이 있음

#  TCP=GO-BACK-N+SR

- TCP는 GBN과 SR방식이 적절히 섞어진 상태

  - GBN처럼 cummulative ACK 방식으로 진행함 - > 가장 작은 시퀀스 넘버와 아직 ACK를 받지 않은 시퀀스 넘버, 윈도우 내에서 다음에 보낼 수 있는 시퀀스 넘버를 관리한다.
  - SR 처럼 1개씩 재전송한다.