# Encapsulation
상위 계층에서 내려온 Service data unit을 한 비트도 건들이지 않고 HEADER만 붙여 하위 계층으로 내려 보내는 형태로 이루어진다. 
이때 HEADER는 계층 간의 프로토콜을 일치시키는 역할을 한다. 
다시 말하자면, 상위 계층에서 내려온 Service data unit과 HEADER가 합쳐져 Protocol data unit을 형성한다. 캡슐화가 이루어지면 사람이 해석할 수 있는 계층은 Application 계층 뿐이다.

- 먼저, User Data는 Application계층으로 전송되어, Application Header와 합쳐져 TCP계층으로 전송된다.
- TCP계층은 Application 계층으로부터 전송 받은 Application Data와 TCP header를 IP계층으로 전송한다.
- IP 계층에서는 상위 계층으로 부터 내려온 TCP segment(TCP HEADER + Application data)를 받아 IP HEADER를 붙여 Network계층으로 전송한다.
- Network계층에서는 IP HEADER, TCP HEADER, Application DATA, Ethernet Trailer가 모두 합쳐져 캡슐화가 이루어진다.

# Decapsulation
Encapsulation과 반대의 형태로 처리된다. HEADER를 통해 Decapsulation을 처리하며, 수신 측 입장에서 데이터를 생성하는 과정이다. 

- 수신 측 입장으로 부터 시작되기 때문에, 이더넷 드라이버로 부터 첫 수신이 이루어진다. 
- 이더넷 드라이버는 해당 데이터를 ARP, IP, RARP 중 어느쪽으로 보낼 지 Ethernet HEADER의 Frame typed을 확인하여 결정한다. 
- IP에서는 TCP, UDP 중 어느 쪽으로 보낼 지를 결정해야 하는데, 이때 사용되는 프로토콜의 HEADER값을 통해 결정한다.
- 마지막 Application 계층에서는 각각의 목적지 포트번호를 확인하여 데이터를 보낸다.
