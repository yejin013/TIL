# 개념

> TCP / IP 4계층이란, 장치들이 인터넷 상에서 데이터를 주고받을 때 쓰는 독립적인 프로토콜의 집합
>

데이터를 보낼 때 TCP/IP 4계층을 거쳐 보낸다.

계층은 ‘**독립적**’이다.

## 4계층

### Application

- SMTP, HTTP/HTTPS, FTP, SSH
- 웹 서비스, 이메일 등 서비스를 실질적으로 사람들에게 제공하는 층

### Transport

- UDP, TCP
- 애플리케이션 계층에서 받은 메시지를 기반으로 세그먼트 또는 데이터그램으로 데이터를 쪼개고 데이터가 오류없이 순서대로 전달되도록 도움을 주는 층

### Internet

- IPv4/IPv6, ICMP, ARP
- 한 노드에서 다른 노드로 전송 계층에서 받은 세그먼트 또는 데이터그램을 패킷화 하여 목적지로 전송하는 역할

### Network Access (= Link)

- 전선, 광섬유, 무선 등으로 데이터가 네트워크를 통해 물리적으로 전송되는 방식

# 캡슐화

> 캡슐화란, 송신자가 수신자에게 데이터를 보낼 때 데이터가 각 계층을 지나며 각 계층의 특징들이 담긴 헤더들이 붙여지는 과정
>
- ex) 전송계층 - TCP 헤더 추가
- ex) 네트워크 계층 - IP 주소 헤더 추가
- Application → Transport → Internet → Link
- 헤더 또는 트레일러가 붙여지면서 계층별 특징이 부여된다.

# 비캡슐화

> 수신자 측에서 캡슐화된 데이터를 역순으로 제거하면서 응용계층까지 도달하는 것
>

# PDU

> PDU (protocol data unit)란, TCP/IP 4계층을 기반으로 설명했을 때 각 계층의 데이터 단위
>
- Application : Message
- Transport : Segment (TCP), Datagram (UDP)
    - Segment : 적절한 크기로 쪼갠 조각
- Internet : Packet
    - Packet : 세그먼트에 SP (Source IP address)와 DP (Destination IP address)가 포함된 IP 헤더가 붙은 형태의 조각 → 주소 정보가 붙게 된다.
- Link : Frame (Data Link Layer), Bit (Physical Layer)
    - Frame : MAC 주소 헤더와 CRC/체크섬 트레일러가 붙은 조각
        - CRC/체크섬 트레일러 : 데이터의 오류 감지를 위한 수학적 함수가 적용된 값. 링크의 오류로 인해 데이터 손상을 감지하는 역할. 오류 검사 할 때 근간이 되는 것이 CRC 알고리즘이며, 이를 기반으로 값을 만들어서 오류 검사 진행.

# OSI 7계층

TCP/IP 4계층과 OSI 7계층과의 차이

- Application Layer = Application Layer + Presentation Layer + Session Layer
- Transport Layer
- Internet Layer = Network Layer
- Network Access Layer = Data-Link Layer + Physical Layer

- 기능은 같다고 봄.
- 용어만 다름

# MTU

> MTU (Maximum Transmission Unit) 이란, 패킷이 쪼개지는 기반. 네트워크 통신할 때 할 수 있는 가장 큰 PDU의 크기

- 일반적으로 1500 바이트
- 네트워크 경로 상에 있는 제일 작은 MTU 크기에 맞게 쪼개져야 전달이 된다.
- 통신을 하는 양쪽 끝은 두 장치의 MTU만이 아니라 중간의 모든 라우터, 스위치, 서버를 고려해야 한다.
- 네트워크 경로 상에 있는 아무 장치나 MTU보다 패킷이 크면 그 패킷은 분할될 수도 있다.
    - 패킷이 분할되지 않는 경우도 있다. 그럴 경우 아예 전달을 하지 않을 수 있다.
    - IPv6는 아예 패킷을 분할하지 않는다.
    - IPv4 헤더에는 flag라는 필드가 있는데 여기서 bit이 1이 되면 “Don’t Fragment” 플래그가 활성화되어 분할이 불가능해진다.
- ping을 통해 MTU 확인 가능
    - ping www.google.com -f -l 1500
    - ping의 경우 IP 헤더 (20바이트) + ICMP 헤더 (8바이트)로 요청이 보내지기 때문에 1472까지 된다.
    - 1500을 1472보다 작은 숫자로 바꾸면 제대로 전송됨을 확인할 수 있다.
- netsh를 통한 MTU 확인
    - nets interface ipv4 show interfaces

# MSS

> MSS (Maximum Segment Size) 란, TCP에서 사용할 수 있는 데이터의 크기이자 TCP 헤더, IP 헤더를 뺀 크기

- 일반적으로 1460 바이트
- MTU가 1500이라도 데이터는 보통 1460 바이트 이하의 크기로 보내야 전달이 된다.

# PMTUD

> PMTUD (Path MTU Discovery)란, 수신자와 송신자의 경로 상에서 장치가 패킷을 누락한 경우 테스트 패킷의 크기를 낮추면서 MTU에 맞게끔 반복해서 보내는 과정
