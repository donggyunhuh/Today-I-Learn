# CS 준비 

## OSI 참조 모델
- 응용 - 표현 - 세션 - 전송 - 네트워크 - 데이터 링크 - 물리

### OSI 참조 모델에서의 데이터 단위
 
### 프로토콜 데이터 단위

- 물리 : 비트
- 데이터 링크 : 프레임
- 네트워크 : 패킷
- 전송 : 세그먼트
- 세션, 표현, 응용 : 메세지

### 서비스 데이터 단위

#### 물리 계층

- RS-232C, X.21
- 리피터 허브

#### 데이터 링크 

- 동기화, 오류제어, 흐름제어
- HDLC, LAPB, MAC, LAPD, PPP
- 랜카드 브리지 스위치

#### 네트워크 계층

- 경로설정, 데이터 교환 및 중계, 트래픽 제어, 패킷 정보 전송 
- x.25, IP
- 라우터

#### 전송 계층

- END to END, 종단 시스템간의 투명한 데이터 전송
- 주소 설정, 다중화, 오류제어, 흐름 제어
- TCP, UDP
- 게이트웨이

#### 세션 계층

- 동기점 사용, 댜화 동기를 위해 전송하는 정보의 일정한 부분애 채크점을 두어 정보의 수신상태를 체크
- 동기점은 오류가 있는 데이터의 회복을 위해 사용
- 토큰

#### 표현 계층
