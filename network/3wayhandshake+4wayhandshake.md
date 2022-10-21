<img src="https://user-images.githubusercontent.com/45252618/197215442-b11b1b09-2991-4f3e-ae67-5da867e584cd.jpeg">
<img src="https://user-images.githubusercontent.com/45252618/197215452-17044434-1eab-4a21-b890-8a151c33098e.jpeg">

3-way-handshake와 4-way-handshake는 TCP connection/disconnection 할 때 진행되는 과정

- **SYN(Synchronization)** : 연결요청, 세션을 설정하는데 사용되며 초기에 시퀀스 번호를 보냄
- **ACK(Acknowledgement)** : 보낸 시퀀스 번호에 TCP 계층에서의 길이 또는 양을 더한 것과 같은 값을 ACK에 포함하여 전송

# 3 way handshake

> 💡 3-way-handshake란, 전송제어 프로토콜(TCP)에서 통신을 하는 장치간 서로 연결이 잘 되어있는지 확인하는 과정/방식

- 송수신 모두 데이터를 주고 받을 준비가 되었다는 것을 보장하고, 실제로 데이터 전달이 시작하기전에 한쪽이 다른 쪽이 준비되었다는 것을 알 수 있도록 한다.

1단계 : A클라이언트는 B서버에 접속을 요청하는 SYN 패킷을 보낸다.

- 송신자가 최초로 데이터를 전송할 때 Sequence Number를 임의의 랜덤 숫자로 지정하고, SYN 플래그 비트를 1로 설정한 세그먼트를 전송한다.
- PORT 상태
    - Client : `CLOSED` → `SYN_SENT` (SYN/ACK 응답 기다리기)
    - Server : `LISTEN`

2단계 : B서버는 SYN요청을 받고 A클라이언트에게 요청을 수락한다는 ACK 와 SYN flag 가 설정된 패킷을 발송하고 A가 다시 ACK으로 응답하기를 기다린다.

- ACK Number필드를 Sequence Number + 1 로 지정하고 SYN과 ACK 플래그 비트를 1로 설정한 새그먼트를 전송한다.
- PORT 상태
    - Client : `CLOSED`
    - Server : `SYN_RCV`

3단계 : A클라이언트는 B서버에게 ACK을 보내고, 이후 연결이 이루어지고 데이터 전송이 이루어진다.

- PORT 상태
    - Client : `ESTABLISED`
    - Server : `SYN_RCV` → ACK → `ESTABLISED`

# 4 way handshake

> 💡 4-way-handshake란, 전송제어 프로토콜(TCP)에서 통신 연결이 되어 있는 장치의 연결을 끊는 과정/방식


1단계 : A클라이언트가 연결 종료를 뜻하는 FIN 플래그를 전송한다.

2단계 : B서버는 FIN 플래그를 받았다는 ACK 확인 메시지를 보내고 자신의 통신이 끝날 때까지 CLOSE_WAIT 상태이다.

- Server(수신자)는 ACK Number 필드를 (Sequence Number + 1)로 지정하고, ACK 플래그 비트를 1로 설정한 세그먼트를 전송한다.
- PORT 상태
    - Server : `CLOSE_WAIT`

3단계 : B서버가 통신이 끝났으면 연결이 종료되었다고 클라이언트에게 FIN플래그를 전송한다.

4단계 : A클라이언트는 확인했다는 메시지를 보낸다. 그러면 연결이 끊기게 된다.

이 때, Server에서 FIN을 전송하기 전에 전송한 패킷이 에러(Routing 지연이나 패킷 유실로 인한 재전송 등으로 인해 FIN패킷보다 늦게 도착하는 상황)가 발생하는 경우, `TIME_WAIT` 상태로 빠지게 된다. 만약 에러로 인해 종료가 지연되거나 타임이 초과되면 `CLOSED`로 들어간다.