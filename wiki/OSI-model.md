# [OSI 모형(Open System Interconnection)](https://ko.wikipedia.org/wiki/OSI_%EB%AA%A8%ED%98%95)

## 정의
OSI 모형(Open Systems Interconnection Reference Model)은 국제표준화기구(ISO)에서 개발한 모델로,
<br>컴퓨터 네트워크 프로토콜 디자인과 통신을 계층으로 나누어 설명한 것이다. 일반적으로 OSI 7 계층 모형이라고 한다.

## 계층 기능
#### [Physical Layer](Physical-Layer)
하드웨어
#### [Data link Layer](Data-link-Layer)
물리계층에서 발생 가능한 오류 찾고 수정에 필요한 수단 제공.<br>
주소 값을 물리적으로 할당 받음.<br>
계층이 없는 단일 구조.
#### [Network Layer](Network-Layer)
주소부여(IP),경로설정(Route).<br>
계층적(hierarchical) 구조.
#### [Transport Layer](Transport-Layer)
연결의 유효성을 제어.<br>
패킷 생성.<br>
#### [Session Layer](Session-Layer)
TCP/IP 세션을 만들고 없애는 책임을 진다.
#### [Presentation Layer](Presentation-Layer)
코드 간의 번역을 담당, 포장/압축/암호화.<br>
예를 들면, EBCDIC로 인코딩된 문서 파일을 ASCII로 인코딩된 파일로 바꿔 주는 것이 표현 계층의 몫이다.
#### [Application Layer](Application-Layer) 
응용 프로세스와 직접 관계하여 일반적인 응용 서비스를 수행.<br>
네트워크 소프트웨어 UI 부분, 사용자의 입출력(I/O)부분.<br>
HTTP, FTP..

## 계층별 예시

|단계|계층|예시| 
|:---|:---|:---|
|7|	응용 계층 | HTTP, SMTP, SNMP, FTP, 텔넷, SSH & Scp, NFS, RTSP |
|6|	표현 계층| XDR, ASN.1, SMB, AFP |
|5|	세션 계층| TLS, SSL, ISO 8327 / CCITT X.225, RPC, 넷바이오스, 애플토크 |
|4|	전송 계층| TCP, UDP, RTP, SCTP, SPX, 애플토크 |
|3|	네트워크 계층| IP, ICMP, IGMP, X.25, CLNP, ARP, RARP, BGP, OSPF, RIP, IPX, DDP |
|2|	데이터 링크 계층| 이더넷, 토큰링, PPP, HDLC, 프레임 릴레이, ISDN, ATM, 무선랜, FDDI |
|1|	물리 계층| 전선, 전파, 광섬유, 동축케이블, 도파관, PSTN, 리피터, DSU, CSU, 모뎀 |
