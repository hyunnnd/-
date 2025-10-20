

## Transport Layer: Overview (전송 계층 개요)

### 🎯 목표 (Our goal)

#### 1. 전송 계층 서비스의 기본 원리 이해

- **Multiplexing, Demultiplexing (다중화, 역다중화)**  
    여러 애플리케이션의 데이터를 하나의 네트워크 연결로 통합하거나, 수신된 데이터를 올바른 애플리케이션으로 분리하는 과정입니다.
- **Reliable Data Transfer (신뢰성 있는 데이터 전송)**  
    전송 중 손실되거나 손상된 데이터를 재전송하여 신뢰성을 보장하는 메커니즘입니다.    
- **Flow Control (흐름 제어)**  
    송신자가 수신자의 처리 속도를 초과하지 않도록 조절하는 기능입니다.
- **Congestion Control (혼잡 제어)**  
    네트워크가 과부하되지 않도록 데이터 전송 속도를 조절하는 기술입니다.

#### 2. 인터넷 전송 계층 프로토콜 학습

- **UDP (User Datagram Protocol)**  
    연결을 설정하지 않는 비연결형 전송 방식 (connectionless transport)
- **TCP (Transmission Control Protocol)**  
    연결을 설정하고 신뢰성 있는 전송을 제공하는 연결지향형 전송 방식 (connection-oriented reliable transport)
- **TCP Congestion Control (TCP 혼잡 제어)**  
    네트워크 혼잡을 방지하기 위한 TCP의 전송 제어 메커니즘


## Transport Services and Protocols (전송 서비스와 프로토콜)

### 1. 전송 계층의 역할

- **논리적 통신(logical communication)** 제공  
    → 서로 다른 호스트에서 실행 중인 **애플리케이션 프로세스 간**의 통신을 가능하게 함.  
    (즉, 실제 물리적 연결이 아니라 논리적으로 종단 간 통신을 제공함)

### 2. 전송 프로토콜의 동작 (end system에서의 동작)

- **송신자(Sender)**
    - 애플리케이션 계층의 메시지를 **세그먼트(segments)** 단위로 분할
    - 분할된 세그먼트를 **네트워크 계층(Network Layer)** 으로 전달

- **수신자(Receiver)**    
    - 네트워크 계층으로부터 받은 **세그먼트들을 재조립(reassemble)** 하여 원래의 메시지 복원
    - 복원된 메시지를 **애플리케이션 계층(Application Layer)** 으로 전달

### 3. 인터넷에서 사용되는 주요 전송 계층 프로토콜

- **TCP (Transmission Control Protocol)**  
    → 연결 지향적, 신뢰성 있는 데이터 전송 제공    
- **UDP (User Datagram Protocol)**  
    → 비연결형, 빠르지만 신뢰성 보장은 없음

**핵심 요약:**  
전송 계층은 네트워크를 통한 실제 데이터 이동이 아닌, **종단 간(end-to-end) 논리적 연결**을 제공하여 애플리케이션 간 메시지 교환이 가능하도록 합니다.


## Transport vs. Network Layer Services and Protocols

### 1. 기본 개념 비교

- **Network layer (네트워크 계층)**  
    → **호스트(host)** 간의 논리적 통신을 제공함.  
    즉, 한 컴퓨터에서 다른 컴퓨터로 데이터가 전송되도록 함.
 
- **Transport layer (전송 계층)**  
    → **프로세스(process)** 간의 논리적 통신을 제공함.  
    즉, 한 호스트의 특정 애플리케이션이 다른 호스트의 특정 애플리케이션과 통신하도록 함.    
    - 전송 계층은 네트워크 계층 서비스를 **의존하며(rely on)**, **확장(enhance)** 함.


### 2. 가정(집) 비유 (Household Analogy)

> “앤(Ann)의 집에 있는 12명의 아이들이 빌(Bill)의 집에 있는 12명의 아이들에게 편지를 보내는 상황”

| 네트워크 개념                    | 비유                              | 설명                                            |
| -------------------------- | ------------------------------- | --------------------------------------------- |
| **Hosts**                  | Houses (집)                      | 두 집이 통신하는 두 개의 호스트에 해당                        |
| **Processes**              | Kids (아이들)                      | 각 집 안에서 통신하는 애플리케이션 프로세스                      |
| **App Messages**           | Letters in envelopes (봉투 안의 편지) | 전송되는 애플리케이션 메시지                               |
| **Transport Protocol**     | Ann과 Bill (편지를 분류하는 사람)         | 각 집 안에서 편지를 올바른 아이(프로세스)에게 전달(demultiplexing) |
| **Network-layer Protocol** | Postal service (우편 서비스)         | 실제 편지를 집에서 집으로 운반하는 역할                        |

---

### 💡 요약

- **네트워크 계층**은 **호스트 간 데이터 전달**을 담당하고,    
- **전송 계층**은 **각 호스트 내부의 프로세스 간 통신**을 가능하게 합니다.
- 즉, 네트워크 계층이 “집에서 집까지” 연결한다면,  
    전송 계층은 “집 안의 사람에서 사람까지” 연결합니다.


## Transport Layer Actions (전송 계층의 동작)

### 📤 Sender (송신자 측 동작)

1. **애플리케이션 계층 메시지를 전달받음**
    - 상위 계층(애플리케이션 계층)에서 생성된 메시지를 입력으로 받음.

2. **세그먼트 헤더 필드 값을 결정함**    
    - 송신지 포트, 수신지 포트, 순서 번호, 확인 응답 번호 등의 **헤더 정보(header fields)** 를 설정함.

3. **세그먼트 생성 (Create segment)**    
    - 결정된 헤더 정보를 메시지 앞에 붙여 **세그먼트(segment)** 를 구성함.
    - 즉,
        Segment=Header+Application Message
4. **세그먼트를 IP 계층으로 전달 (Pass to IP)**
    - 완성된 세그먼트를 **네트워크 계층(Network layer, IP)** 로 넘겨 전송을 요청함.

### 💡 요약

> 전송 계층은 애플리케이션 계층으로부터 메시지를 받아  
> 이를 전송 가능한 형태인 **세그먼트(Segment)** 로 변환하고,  
> 이 세그먼트를 **IP 계층에 전달**하여 실제 전송을 수행하도록 합니다.


## Transport Layer Actions (전송 계층의 동작)

### 📥 Receiver (수신자 측 동작)

1. **IP 계층으로부터 세그먼트를 수신함**
    - 네트워크 계층(IP)에서 전달된 **세그먼트(segment)** 를 입력으로 받음.

2. **헤더 값을 확인함 (Check header values)**    
    - 송신지 포트, 수신지 포트, 순서 번호, 오류 검증 등 **헤더 필드(header fields)** 정보를 검사함.

3. **애플리케이션 계층 메시지를 추출함 (Extract message)**    
    - 세그먼트에서 **헤더를 제거**하고, 원래의 **애플리케이션 메시지(application-layer message)** 를 복원함.

4. **소켓(socket)을 통해 애플리케이션으로 전달함 (Demultiplexing)**    
    - 수신 포트 번호를 이용해 **올바른 애플리케이션 프로세스**로 메시지를 전달함.
    - 이 과정을 **역다중화(demultiplexing)** 라고 함.

### 💡 요약

> 수신 측 전송 계층은 IP로부터 세그먼트를 받아  
> 헤더를 검사하고 메시지를 추출한 뒤,  
> 해당 메시지를 올바른 **소켓(프로세스)** 으로 전달합니다.


## Two Principal Internet Transport Protocols

### 1. **TCP (Transmission Control Protocol)**

- **신뢰성 있는, 순서 보장 전송 (reliable, in-order delivery)**  
    → 데이터 손실 없이, 전송 순서를 유지하여 전달함.
- **혼잡 제어 (Congestion control)**  
    → 네트워크 혼잡을 감지하고 전송 속도를 조절함.
- **흐름 제어 (Flow control)**  
    → 송신 측이 수신 측의 처리 속도를 초과하지 않도록 제어함.
- **연결 지향형 (Connection-oriented)**  
    → 통신 전, **연결 설정 과정(setup)** 이 필요함.

### 2. **UDP (User Datagram Protocol)**

- **비신뢰성, 무순서 전송 (unreliable, unordered delivery)**  
    → 패킷 손실, 중복, 순서 뒤바뀜이 발생할 수 있음.    
- **비연결형 (Connection-less)**  
    → 연결 설정 과정이 필요하지 않음.
- **단순한(best-effort) IP의 확장**  
    → IP 프로토콜을 기반으로 하되, 추가적인 제어 기능이 거의 없음.  
    → 오버헤드가 작고 빠르지만, 신뢰성은 제공하지 않음.

### 3. **전송 계층에서 제공하지 않는 서비스**

- **지연 보장 (Delay guarantees)** 없음    
- **대역폭 보장 (Bandwidth guarantees)** 없음

즉, 전송 계층은 “가능한 한 최선을 다하는(best-effort)” 전송만 제공합니다.

### 💡 요약

| 항목    | TCP                           | UDP               |
| ----- | ----------------------------- | ----------------- |
| 연결 방식 | 연결 지향형                        | 비연결형              |
| 신뢰성   | 높음 (재전송, 순서 보장)               | 낮음 (손실 가능)        |
| 속도    | 느림 (제어 많음)                    | 빠름 (단순 구조)        |
| 사용 예시 | 웹(HTTP), 이메일(SMTP), 파일전송(FTP) | 스트리밍, DNS, VoIP 등 |


## Multiplexing / Demultiplexing

### 📤 Multiplexing at Sender (송신 측 다중화)

- 여러 개의 **소켓(socket)** 으로부터 데이터를 수집함.
- 각 데이터에 **전송 계층 헤더(transport header)** 를 추가함.  
    → 이 헤더는 나중에 **역다중화(demultiplexing)** 과정에서 사용됨.
- 즉, 송신 측은 여러 애플리케이션 프로세스에서 온 데이터를 **하나의 네트워크 연결로 묶어 전송**함.

### 📥 Demultiplexing at Receiver (수신 측 역다중화)

- 수신 측 전송 계층은 IP 계층으로부터 **세그먼트(segment)** 를 받음.    
- **헤더 정보(header info)** 를 참조하여,  
    해당 세그먼트를 **올바른 소켓(socket)** 으로 전달함.
- 즉, 수신 측은 하나의 연결로 들어온 여러 데이터 흐름을 **각 애플리케이션 프로세스로 분배**함.

### 💡 개념 요약

| 구분                 | 역할   | 주요 동작                            |
| ------------------ | ---- | -------------------------------- |
| **Multiplexing**   | 송신 측 | 여러 소켓의 데이터를 하나의 연결로 통합하여 전송      |
| **Demultiplexing** | 수신 측 | 수신된 세그먼트를 헤더 정보를 이용해 올바른 소켓으로 분배 |

### 🧩 추가 개념

- **Socket (소켓)**:  
    애플리케이션 프로세스가 전송 계층과 통신하기 위한 인터페이스.    
- **Process (프로세스)**:  
    실행 중인 애플리케이션의 인스턴스.

요약하자면,

> 전송 계층은 송신 측에서 여러 프로세스의 데이터를 하나로 묶고(multiplexing),  
> 수신 측에서 이를 다시 분리하여 각 프로세스에 전달(demultiplexing)합니다.


## How Demultiplexing Works

### 🧩 1. IP 데이터그램 수신 과정

- 호스트(host)는 **IP 데이터그램(datagram)** 을 수신함.
- 각 데이터그램에는 다음 정보가 포함되어 있음:
    - **출발지 IP 주소 (Source IP Address)**
    - **목적지 IP 주소 (Destination IP Address)**

- 각 데이터그램은 **하나의 전송 계층 세그먼트(transport-layer segment)** 를 포함함.    
- 각 세그먼트에는 다음 정보가 포함됨:
    - **출발지 포트 번호 (Source Port Number)**
    - **목적지 포트 번호 (Destination Port Number)**

### ⚙️ 2. 세그먼트의 전달 과정

- 호스트는 **IP 주소와 포트 번호(IP addresses & port numbers)** 를 사용하여  
    수신된 세그먼트를 **올바른 소켓(socket)** 으로 전달함.    
- 즉,
    (Source IP, Source Port, Destination IP, Destination Port)
    조합을 이용하여 해당 세그먼트가 어떤 프로세스의 데이터인지 식별함.

### 📦 3. TCP/UDP 세그먼트 구조

| 필드                             | 설명                  |
| ------------------------------ | ------------------- |
| **Source Port #**              | 송신 측 포트 번호          |
| **Destination Port #**         | 수신 측 포트 번호          |
| **Other Header Fields**        | 순서 번호, 체크섬, 제어 비트 등 |
| **Application Data (Payload)** | 실제 애플리케이션 계층의 데이터   |

> 포트 번호(Port number)는 전송 계층이 어떤 애플리케이션 프로세스로 데이터를 보낼지 결정하는 핵심 정보입니다.

### 💡 요약

- **IP 계층:** 호스트 간 데이터 전달    
- **전송 계층:** 포트 번호를 기반으로 프로세스 간 데이터 전달
- **Demultiplexing:**  
    IP + 포트 정보를 이용해 세그먼트를 올바른 소켓으로 라우팅하는 과정


## Connectionless Demultiplexing

### 🔹 개념 요약 (Recall)

#### 1. 소켓 생성 시

- 소켓(socket)을 생성할 때, **호스트 내 고유 포트 번호(host-local port #)** 를 지정해야 함.
    `DatagramSocket mySocket1 = new DatagramSocket(12534);`
    → 위 예시는 포트 번호 **12534**를 사용하는 UDP 소켓을 생성한 예입니다.

#### 2. 데이터그램 송신 시

- UDP 소켓을 통해 데이터그램을 보낼 때는 다음을 지정해야 함:
    - **목적지 IP 주소 (Destination IP address)**
    - **목적지 포트 번호 (Destination port #)**

### 🔹 수신 과정 (Receiving Host)

- 수신 호스트가 **UDP 세그먼트**를 받으면 다음을 수행함:    
    1. 세그먼트 헤더에서 **목적지 포트 번호(destination port #)** 를 확인함.
    2. 해당 포트 번호를 사용하는 **소켓(socket)** 으로 세그먼트를 전달함.

### 🔹 중요한 특징

- **같은 목적지 포트 번호(dest. port #)** 를 가지더라도,  
    **송신 IP 주소(Source IP address)** 와 **송신 포트 번호(Source port #)** 가 서로 다른 여러 데이터그램들은  
    **모두 동일한 소켓(same socket)** 으로 전달됩니다.    

> 즉, UDP는 송신지 구분 없이 “목적지 포트 번호”만으로 수신 소켓을 식별합니다.

### 💡 정리

| 항목         | 설명                                         |
| ---------- | ------------------------------------------ |
| 연결 방식      | **비연결형 (Connectionless)**                  |
| 세그먼트 구분 기준 | **목적지 포트 번호 (Destination Port #)**         |
| 특징         | 송신지 IP/포트가 달라도 동일한 목적지 포트를 가지면 같은 소켓으로 수신됨 |
| 예시         | DNS, DHCP 등 UDP 기반 서비스                     |

요약하자면,

> UDP의 역다중화는 **“단순히 포트 번호 기반”** 으로 작동하며,  
> TCP처럼 송수신지 주소 쌍 전체를 이용한 연결 구분을 하지 않습니다.



## Connectionless Demultiplexing: An Example

### 🧩 개요

이 예시는 **UDP의 비연결형(Connectionless)** 통신에서  
송신자와 수신자 간에 **포트 번호(port number)** 를 이용해  
데이터그램이 어떻게 전달(demultiplexing)되는지를 보여줍니다.

### ⚙️ UDP 소켓 생성 예시

|호스트|코드 예시|포트 번호|프로세스|
|---|---|---|---|
|송신자 A|`DatagramSocket mySocket2 = new DatagramSocket(9157);`|9157|P3|
|서버 (수신자)|`DatagramSocket serverSocket = new DatagramSocket(6428);`|6428|P1|
|송신자 B|`DatagramSocket mySocket1 = new DatagramSocket(5775);`|5775|P4|

### 📤 송신 측 (Senders)

- **P3** 프로세스는 자신의 소켓(포트 9157)을 통해  
    **목적지 포트 6428** (서버의 포트)로 UDP 데이터그램을 전송합니다.
    - 세그먼트 정보:
        `source port: 9157 dest port: 6428`

- **P4** 프로세스도 동일하게 서버의 포트 6428로 데이터그램을 전송합니다.    
    - 세그먼트 정보:
        `source port: 5775 dest port: 6428`

### 📥 수신 측 (Server, P1)

- 서버는 포트 **6428**에서 모든 UDP 데이터그램을 수신함.    
- 수신된 데이터그램의 **출발지 IP 주소와 포트 번호**는 다를 수 있으나,  
    **모두 동일한 목적지 포트(6428)** 를 향하므로  
    → **하나의 동일한 소켓(serverSocket)** 으로 전달됨.

### 💡 핵심 요약

| 특징    | 설명                                          |
| ----- | ------------------------------------------- |
| 연결 방식 | **비연결형(Connectionless)** — 송신 전 연결 설정 없음    |
| 식별 기준 | **목적지 포트 번호(dest port)** 만 이용               |
| 결과    | 송신자가 여러 명이어도, 목적지 포트가 같다면 모두 **같은 소켓으로 수신** |
| 장점    | 단순하고 빠름                                     |
| 단점    | 송신자 구분 불가, 신뢰성 부족                           |


### 📘 예시 정리

| 송신자          | Source Port | Destination Port | 수신 소켓               |
| ------------ | ----------- | ---------------- | ------------------- |
| P3 (왼쪽 호스트)  | 9157        | 6428             | serverSocket (6428) |
| P4 (오른쪽 호스트) | 5775        | 6428             | serverSocket (6428) |

> 따라서 서버는 두 개의 다른 송신자로부터 온 데이터그램을  
> **같은 소켓(serverSocket)** 을 통해 수신합니다.


## Connection-oriented Demultiplexing

### 🔹 1. TCP 소켓의 식별 방식

- **TCP 소켓(TCP socket)** 은 다음의 **4가지 정보(4-tuple)** 로 식별됩니다.    
    1. **Source IP address (출발지 IP 주소)**
    2. **Source port number (출발지 포트 번호)**
    3. **Destination IP address (목적지 IP 주소)**
    4. **Destination port number (목적지 포트 번호)**

### 🔹 2. Demultiplexing (역다중화)

- 수신 측(receiver)은 위의 **4-tuple 전체 정보**를 사용하여  
    세그먼트를 **올바른 TCP 소켓**으로 전달합니다.    
    > 즉, 단순히 목적지 포트만 보는 UDP와 달리,  
    > TCP는 송신자와 수신자의 주소/포트를 모두 고려하여 구분함.

### 🔹 3. 서버의 다중 연결 처리

- 서버는 동시에 여러 개의 TCP 소켓을 지원할 수 있습니다.    
    - 각 소켓은 **고유한 4-tuple** 로 식별됩니다.
    - 각 소켓은 **서로 다른 클라이언트 연결**에 대응됩니다.

예시:

| 클라이언트 IP     | 클라이언트 포트 | 서버 IP       | 서버 포트 | 결과   |
| ------------ | -------- | ----------- | ----- | ---- |
| 192.168.1.10 | 5050     | 203.0.113.5 | 80    | 소켓 A |
| 192.168.1.11 | 5051     | 203.0.113.5 | 80    | 소켓 B |
| 192.168.1.12 | 5052     | 203.0.113.5 | 80    | 소켓 C |

→ **동일한 서버 포트(80)** 를 사용하더라도,  
서버는 **클라이언트의 IP·포트 쌍**을 통해 각 연결을 구분함.

### 💡 비교 요약

| 구분          | UDP (Connectionless) | TCP (Connection-oriented) |
| ----------- | -------------------- | ------------------------- |
| 식별 기준       | 목적지 포트 번호            | 4-tuple (출발지/목적지 IP + 포트) |
| 연결 유지       | 없음                   | 있음 (연결 설정 필요)             |
| 다중 클라이언트 구분 | 불가                   | 가능                        |
| 신뢰성         | 낮음                   | 높음 (순서·재전송 보장)            |


요약하자면,
> **UDP**는 단일 포트 기준의 단순 역다중화,  
> **TCP**는 네트워크 종단점 전체(4-tuple)에 기반한 **정확한 연결 구분**을 수행합니다.


## Connection-oriented Demultiplexing: Example

### 🧩 시나리오 개요

- **서버 (IP 주소 B)** 는 Apache HTTP 서버를 실행 중이며,  
    **포트 80**에서 클라이언트 요청을 수신함.    
- **세 개의 TCP 세그먼트**가 모두 **서버 B의 포트 80**으로 도착하지만,  
    각각 **다른 클라이언트(host)** 로부터 전송되었음.

### ⚙️ 연결 정보 (4-tuple)

| 구분   | 출발지 IP (Source IP) | 출발지 Port | 목적지 IP (Destination IP) | 목적지 Port | 서버 소켓 (수신) |
| ---- | ------------------ | -------- | ----------------------- | -------- | ---------- |
| 연결 ① | A                  | 9157     | B                       | 80       | P4         |
| 연결 ② | C                  | 5775     | B                       | 80       | P5         |
| 연결 ③ | C                  | 9157     | B                       | 80       | P6         |

### 📥 서버의 역다중화 (Demultiplexing)

- 서버 B는 **세 개의 세그먼트 모두 동일한 목적지 포트(80)** 로 수신하지만,  
    각 세그먼트의 **출발지 IP/포트 조합이 다름**.    
- 따라서 서버는 다음과 같은 **4-tuple**을 이용해  
    각 세그먼트를 **다른 소켓**으로 분리(demultiplexing)함.

(Source IP, Source Port, Destination IP, Destination Port)

- 결과적으로:
    - (A, 9157, B, 80) → 소켓 P4
    - (C, 5775, B, 80) → 소켓 P5
    - (C, 9157, B, 80) → 소켓 P6

### 💡 핵심 요약

| 항목    | 내용                                                  |
| ----- | --------------------------------------------------- |
| 연결 방식 | TCP (연결지향형)                                         |
| 식별 기준 | 4-tuple (출발지/목적지 IP + 포트)                           |
| 특징    | 같은 목적지 포트라도 서로 다른 클라이언트 연결을 개별 소켓으로 구분 가능           |
| 장점    | 다중 클라이언트 동시 연결 지원 (예: 여러 사용자가 같은 웹 서버 포트 80에 접속 가능) |


### 🧠 정리 문장

> “모든 세그먼트가 동일한 목적지 IP(B)와 포트(80)을 향하지만,  
> 서버는 각 클라이언트의 IP와 포트 조합(4-tuple)을 통해  
> 서로 다른 소켓으로 정확히 분리(demultiplex)한다.”



###### ChatGPT의 말:

## 요약 (Summary)

- 다중화(Multiplexing)와 역다중화(Demultiplexing)는 세그먼트 또는 데이터그램의 **헤더 필드 값**(예: 포트 번호, IP 주소 등)에 기반하여 이루어진다.
- **UDP:** 목적지 포트 번호만(destination port number only)을 이용하여 역다중화를 수행한다.
- **TCP:** 출발지와 목적지의 IP 주소 및 포트 번호로 이루어진 **4-튜플(4-tuple)** 을 이용하여 역다중화를 수행한다.
- 다중화와 역다중화 과정은 **모든 계층(all layers)** 에서 발생한다.


## UDP: 사용자 데이터그램 프로토콜 (User Datagram Protocol)

- **“no frills”, “bare bones”**: 최소한의 기능만 제공하는 인터넷 전송 프로토콜

- **“best effort” 서비스**: UDP 세그먼트는 다음과 같은 문제가 발생할 수 있음
    - 손실될 수 있음 (lost)        
    - 순서가 뒤바뀐 채로 애플리케이션에 도착할 수 있음 (delivered out-of-order)

- **비연결형(connectionless)**
    - 송신자와 수신자 간 **핸드셰이킹(handshaking)** 과정이 없음        
    - 각 UDP 세그먼트는 **서로 독립적으로 처리됨**

### 왜 UDP가 존재하는가? (Why is there a UDP?)

- **연결 설정(connection establishment)** 과정이 없음 → RTT 지연이 추가되지 않음
- **단순성(simple)**: 송신자와 수신자 모두 **연결 상태(state)** 를 유지하지 않음
- **헤더 크기가 작음 (small header size)**
- **혼잡 제어 없음 (no congestion control)**
    - UDP는 원하는 속도로 데이터를 전송할 수 있음
    - 네트워크 혼잡 상황에서도 동작 가능


## UDP: 사용자 데이터그램 프로토콜 (User Datagram Protocol)

- **UDP의 사용 예시:**
    - **스트리밍 멀티미디어 애플리케이션** (손실 허용, 전송 속도에 민감함)
    - **DNS (Domain Name System)**
    - **SNMP (Simple Network Management Protocol)**
    - **HTTP/3**


- **UDP 위에서 신뢰성 있는 전송이 필요한 경우** (예: HTTP/3):    
    - 필요한 **신뢰성(reliability)** 을 **애플리케이션 계층(application layer)** 에서 추가함
    - **혼잡 제어(congestion control)** 또한 **애플리케이션 계층** 에서 구현함



## UDP: 전송 계층의 동작 (Transport Layer Actions)

### 🔹 UDP 송신자(Udp Sender)의 동작 과정

1. **애플리케이션 계층 메시지를 전달받음**
    - 상위 계층(예: SNMP 클라이언트)에서 생성된 메시지를 입력으로 받음.

2. **UDP 세그먼트 헤더 필드 값을 결정함**    
    - 송신 포트 번호, 수신 포트 번호, 세그먼트 길이, 체크섬 등을 설정함.

3. **UDP 세그먼트를 생성함**    
    - 헤더(UDP Header)와 애플리케이션 메시지를 결합하여 UDP 세그먼트를 만듦.
        `UDP 세그먼트 = UDP 헤더 + 애플리케이션 메시지`

4. **세그먼트를 IP 계층으로 전달함**    
    - 완성된 세그먼트를 네트워크 계층(IP)에 전달하여 전송이 이루어짐.

### 💡 예시

- **SNMP 클라이언트 → SNMP 서버**    
    - 클라이언트는 SNMP 메시지를 UDP를 통해 전송함.
    - UDP 계층은 메시지에 헤더를 추가하고 IP 계층으로 넘김.        
    - 서버 측 UDP 계층은 세그먼트를 수신 후, SNMP 애플리케이션으로 전달함.


## UDP: 전송 계층의 동작 (Transport Layer Actions)

### 🔹 UDP 수신자(Receiver)의 동작 과정

1. **IP 계층으로부터 세그먼트를 수신함**
    - 네트워크 계층(IP)이 전달한 UDP 세그먼트를 받아 처리 시작.

2. **UDP 헤더의 체크섬(checksum) 값을 확인함**    
    - 전송 중 오류가 발생했는지 검사하여 데이터의 무결성을 확인함.

3. **애플리케이션 계층 메시지를 추출함**    
    - UDP 헤더를 제거하고, 순수한 애플리케이션 계층 메시지(SNMP msg 등)를 복원함.

4. **소켓(socket)을 통해 해당 애플리케이션으로 전달함**    
    - 목적지 포트 번호를 확인하여 올바른 소켓으로 메시지를 분배(demultiplexing)함.

### 💡 예시

- **SNMP 서버 → SNMP 클라이언트**    
    - 서버가 보낸 SNMP 메시지가 UDP 세그먼트 형태로 클라이언트에 도착함.
    - 클라이언트의 UDP 계층이 체크섬을 검사하고, 메시지를 추출한 후  
        해당 SNMP 애플리케이션으로 전달함.


## UDP 세그먼트 헤더 (UDP Segment Header)

UDP 세그먼트는 **헤더(8바이트)** 와 **데이터(페이로드)** 로 구성됩니다.

### 🔹 헤더 구조 (총 4개 필드, 각 16비트씩)

1. **Source Port # (송신 포트 번호)**    
    - 데이터를 보낸 프로세스(애플리케이션)의 포트 번호입니다.
    - 응답을 받을 때 어떤 프로세스로 돌려보낼지 구분하는 역할을 합니다.

2. **Destination Port # (수신 포트 번호)**    
    - 데이터를 받을 대상 프로세스의 포트 번호입니다.
    - 수신 측에서 해당 포트 번호를 사용하여 올바른 소켓으로 데이터를 전달합니다.

3. **Length (길이)**    
    - UDP 세그먼트 전체의 길이를 바이트 단위로 표시합니다.
    - 헤더(8바이트) + 데이터(payload)의 총합입니다.

4. **Checksum (체크섬)**    
    - 데이터 전송 중 발생할 수 있는 오류를 검출하기 위한 값입니다.
    - 송신 측에서 계산하여 헤더에 포함시키고, 수신 측에서 다시 계산하여 비교함으로써 데이터의 무결성을 확인합니다.

### 🔹 Application Data (Payload)

- 애플리케이션 계층에서 전달된 실제 데이터입니다.    
- 예: DNS 질의, SNMP 메시지, 오디오/비디오 스트리밍 데이터 등.

✅ **정리:**  
UDP 세그먼트 = [헤더(8바이트) + 애플리케이션 데이터]  
헤더는 송·수신 포트 번호, 세그먼트 길이, 체크섬으로 구성되어 신속하지만 최소한의 기능만 제공합니다.


## UDP 체크섬 (UDP Checksum)

### 🎯 **목표 (Goal)**

전송된 세그먼트에서 **오류(즉, 비트 반전 등)** 가 발생했는지를 검출하는 것입니다.

### 🔹 **동작 원리**

1. **송신 측 (Transmitter)**    
    - 데이터를 16비트 단위로 나눠서 모두 더한 후,  
        그 합계를 **1의 보수(Complement)** 로 바꿔 **체크섬(checksum)** 으로 기록합니다.
    - 예시:
        `첫 번째 숫자: 5 두 번째 숫자: 6 합계: 11 → 송신 체크섬 = 11 (예시)`

2. **수신 측 (Receiver)**    
    - 수신한 데이터의 각 숫자를 다시 더한 뒤, **체크섬을 포함하여 다시 합산**합니다.
    - 이 합이 **모두 1(정상)** 이어야 오류가 없다고 판단합니다.
    - 예시에서, 데이터가 전송 중 변형되어
        `받은 값: 4, 6 → 합계: 10 ≠ 송신 측 체크섬(11)`
        이므로 **오류 발생**으로 감지됩니다.

### 🔹 **핵심 개념 요약**

| 단계  | 주체    | 동작              | 목적          |
| --- | ----- | --------------- | ----------- |
| ①   | 송신자   | 데이터를 더해 체크섬 생성  | 오류 검출 정보 생성 |
| ②   | 수신자   | 받은 데이터와 체크섬 합산  | 무결성 검증      |
| ③   | 비교 결과 | 같으면 정상 / 다르면 오류 | 전송 오류 탐지    |

✅ **결론:**  
UDP 체크섬은 **데이터가 손상되었는지 확인하는 기본적인 오류 검출 메커니즘**입니다.  
일부 손상은 잡지 못할 수도 있지만, 간단하고 빠른 무결성 검증 기능을 제공합니다.


## UDP 체크섬 (UDP Checksum)

### 🔹 **송신자 (Sender) 측 동작**

1. **UDP 세그먼트의 전체 내용을 16비트 단위 정수열로 간주함**
    - UDP 헤더, 데이터, 그리고 IP 주소 일부를 포함합니다.

2. **체크섬 계산**    
    - 각 16비트 숫자를 모두 더하고, 그 합의 **1의 보수(One’s Complement)** 값을 취합니다.
    - 이 결과가 체크섬(Checksum) 값이 됩니다.

3. **체크섬을 UDP 헤더의 체크섬 필드에 저장함**    
    - 이 값은 수신 측이 동일하게 계산했을 때 비교 기준이 됩니다.

### 🔹 **수신자 (Receiver) 측 동작**

1. **수신한 세그먼트의 체크섬 계산**    
    - 받은 데이터 전체를 16비트 단위로 더하고, 1의 보수를 취해 새 체크섬을 계산합니다.

2. **송신자 체크섬과 비교**    
    - 계산된 체크섬과 UDP 헤더에 포함된 체크섬 필드 값을 비교합니다.
    - 두 값이 **같으면 오류 없음**,  
        **다르면 오류 발생(error detected)** 으로 간주합니다.
### ⚠️ **주의사항**

- 체크섬이 같다고 해서 **오류가 완전히 없다고 보장되지는 않습니다.**  
    (일부 비트 반전 조합은 우연히 같은 체크섬을 가질 수도 있습니다.)    
- 그러나 UDP는 빠른 전송이 중요하므로, 간단한 이 방식으로 충분한 오류 검출 기능을 제공합니다.

✅ **요약:**  
UDP 체크섬은 16비트 단위의 합과 1의 보수를 이용하여  
전송 중 데이터가 손상되었는지를 **간단하고 효율적으로 검사하는 방법**입니다.


## 인터넷 체크섬 예시 (Internet Checksum: an example)

### 🔹 **예제: 두 개의 16비트 정수 더하기**

`1 1 1 0 0 1 1 0 0 1 1 0 1 0 1 0`   
`1 1 0 1 0 1 0 1 0 1 0 1 0 1 0 1`  

### 🔸 **1단계: 16비트 덧셈 수행**

두 16비트 숫자를 **이진수 덧셈**으로 더합니다.

- 만약 **가장 왼쪽 비트(최상위 비트)** 에서 자리올림(carry)이 발생하면,  
    그 비트는 **wraparound(맨 오른쪽)** 으로 다시 더해줍니다.

`wraparound → 1 1 0 1 1 1 0 1 1 1 1 0 1 1 1 0 0`

### 🔸 **2단계: 1의 보수(One’s Complement) 취하기**

모든 비트를 반전시켜 **체크섬(checksum)** 을 계산합니다.

`sum        : 1 0 1 1 1 0 1 1 1 1 0 1 1 1 0 0`   
`checksum   : 0 1 0 0 0 1 0 0 0 0 1 0 0 0 1 1`  

### 🔹 **결과 해석**

- **체크섬(checksum)** 은 송신 측에서 계산되어 UDP 헤더에 포함됩니다.    
- **수신 측**은 데이터를 모두 더한 뒤 1의 보수를 취해 비교합니다.
    - 같으면 → 오류 없음
    - 다르면 → 전송 중 오류 발생

### ⚙️ **참고 (Note)**

> 숫자를 더할 때 최상위 비트에서 자리올림(carryout)이 발생하면,  
> 그 값을 다시 결과의 최하위 비트에 더해야 합니다.  
> 이것이 **wraparound 덧셈** 방식입니다.

✅ **정리:**  
인터넷 체크섬은 데이터를 16비트 단위로 더하고, 자리올림을 순환시킨 후  
결과의 1의 보수를 취함으로써 **간단한 오류 검출 기능**을 수행합니다.


## UDP 요약 (Summary: UDP)

### 🔹 **“No frills” 프로토콜 (단순한 프로토콜)**

- **세그먼트 손실 또는 순서 뒤바뀜 가능**  
    → UDP는 신뢰성을 보장하지 않으므로 데이터가 손실되거나 순서가 바뀌어 도착할 수 있습니다.
- **Best effort service (최선형 서비스)**  
    → “보내고 잘 도착하길 바란다(Send and hope for the best)” 방식으로 동작합니다.

### 🔹 **UDP의 장점 (UDP has its plusses)**

1. **연결 설정(Setup) 또는 핸드셰이킹(Handshaking) 불필요**
    - 연결 확립 과정이 없으므로 **RTT(왕복 지연 시간)** 발생이 없습니다.        
    - 빠른 전송이 가능합니다.

2. **네트워크 서비스가 불안정해도 동작 가능**
    - TCP처럼 복잡한 흐름 제어나 혼잡 제어가 없으므로  
        일부 구간이 불안정해도 계속 데이터 전송이 가능합니다.

3. **체크섬(Checksum)을 통한 기본적인 신뢰성 확보**    
    - 데이터 손상 여부를 검출할 수 있는 최소한의 오류 검출 기능을 제공합니다.

### 🔹 **응용 계층에서 기능 확장 가능**

- UDP 위에 추가적인 신뢰성 기능이나 제어 기능을 구현할 수 있습니다.  
    예: **HTTP/3** (QUIC 프로토콜 기반)  
    → UDP를 기반으로 하되, 응용 계층에서 신뢰성과 혼잡 제어를 추가 구현함.    

✅ **요약:**  
UDP는 단순하고 빠르며, 최소한의 오버헤드로 동작하는 **비연결형 전송 프로토콜**입니다.  
다만, 신뢰성·순서·혼잡 제어를 지원하지 않으므로, 필요한 경우 **응용 계층에서 직접 보완해야 합니다.**


part 2

# Principles of Reliable Data Transfer

## 핵심 개념

- **신뢰성 전송 프로토콜(reliable data transfer protocol)**의  복잡도**는  
    **비신뢰적 채널(unreliable channel)**의 특성에 따라 달라집니다.
- 송신자(sender)와 수신자(receiver)는 **서로의 상태(state)**를 **알지 못함**.  
    예: “메시지가 제대로 수신되었는가?”를 모름.
- 단, **메시지를 통해 상태를 알려주지 않는 한** 서로의 상태를 알 수 없음.
## 채널 특성 예시

- 데이터 **손실(loss)**    
- 데이터 **손상(corruption)**
- 데이터 **순서 변경(reordering)**


# Reliable Data Transfer: Getting Started

## 개요

- 송신자와 수신자 측의 **신뢰성 전송 프로토콜(rdt)** 을 **단계적으로(incrementally)** 개발함.
- **단방향 데이터 전송(unidirectional transfer)**만 고려하지만,  
    **제어 정보(control info)**는 양방향으로 흐름.
- 송신자와 수신자를 **유한 상태 기계(FSM, Finite State Machine)**로 표현함.

## FSM (유한 상태 기계) 개념

- **state (상태):** 현재 상황을 나타내며, 다음 상태는 발생하는 **이벤트(event)**에 의해 결정됨.   
- **event:** 상태 변화를 일으키는 사건.
- **actions:** 상태 전이 시 수행되는 동작.


# Reliable Data Transfer Protocol (rdt): Interfaces

## 주요 함수 요약

- **`rdt_send()`**    
    - 상위 계층(예: 애플리케이션)에서 호출됨.
    - 송신 측에서 데이터를 수신 측으로 전달하기 위해 사용.

- **`udt_send()`**    
    - `rdt` 내부에서 호출되어 **비신뢰적 채널(unreliable channel)**을 통해 패킷을 전송함.

- **`rdt_rcv()`**    
    - 수신 측에서 패킷이 도착했을 때 호출됨.
    - 수신된 데이터를 처리 후 상위 계층에 전달.

- **`deliver_data()`**    
    - `rdt`가 수신 데이터를 상위 계층(예: 애플리케이션)으로 넘겨줄 때 호출됨.

## 데이터 흐름

**송신 측:**  
`application → rdt_send() → udt_send() → unreliable channel`

**수신 측:**  
`unreliable channel → rdt_rcv() → deliver_data() → application`


# rdt1.0: Reliable Transfer over a Reliable Channel

## 핵심 내용

- 기본 채널이 **완전히 신뢰할 수 있음**
    - 비트 오류 없음        
    - 패킷 손실 없음

- 송신자(sender)와 수신자(receiver)는 **각각 독립적인 FSM(유한 상태 기계)** 사용
    - 송신자: 상위 계층에서 받은 데이터를 채널로 전송
    - 수신자: 채널에서 데이터를 읽어 상위 계층으로 전달        

## Sender

1. Wait for call from above
2. `rdt_send(data)`
3. `packet = make_pkt(data)`    
4. `udt_send(packet)`

## Receiver

1. Wait for call from below
2. `rdt_rcv(packet)`
3. `extract(packet, data)`    
4. `deliver_data(data)`


✅ 오류나 손실이 없으므로 **가장 단순한 형태의 rdt**, 즉 **rdt1.0** 구조임.


# rdt2.0: Channel with Bit Errors

## 핵심 내용

- 채널이 완벽하지 않아, **패킷 내의 비트가 바뀔 수 있음(bit flip)**.    
- **체크섬(checksum)** 사용으로 비트 오류를 탐지함  
    (예: Internet checksum).
- 핵심 질문:  
    → **오류가 발생했을 때, 어떻게 복구할 것인가?**

## 참고 비유

> 사람은 대화 중 오류가 발생하면 어떻게 복구할까?  
> → “다시 말해 주세요”, “뭐라고요?” 와 같은 **확인 및 재전송 요청(ACK/NACK 개념)** 을 통해 복구함.

✅ rdt2.0은 오류 감지(checksum)와 **오류 복구(retransmission)** 개념을 도입하는 단계임.


# rdt2.0: Channel with Bit Errors

## 핵심 내용

- 채널에서 **비트 오류(bit error)**가 발생할 수 있음.    
- **체크섬(checksum)**을 사용하여 오류를 탐지함.
- 문제: 오류를 어떻게 복구할 것인가?

## 오류 복구 방법

- **ACK (Acknowledgement):**  
    수신자가 “패킷이 정상적으로 도착했다”고 송신자에게 알림.    
- **NAK (Negative Acknowledgement):**  
    수신자가 “패킷에 오류가 있다”고 송신자에게 알림.
- **송신자(Sender):**  
    NAK를 받으면 해당 패킷을 **재전송(retransmit)** 함.

## 전송 방식

**Stop and Wait**

- 송신자는 **한 번에 하나의 패킷만 전송**하고,  
    수신자의 응답(ACK/NAK)을 **기다린 후 다음 패킷 전송**.
 

✅ rdt2.0은 **비트 오류 탐지(Checksum)** + **ACK/NAK 기반 재전송**을 통해 신뢰성을 보장하는 모델임.


# rdt2.0: FSM Specification

## Sender FSM

- **State 1: Wait for call from above**
    - `rdt_send(data)` 호출 → `sndpkt = make_pkt(data, checksum)`
    - `udt_send(sndpkt)` → 패킷 전송
    - 이후 **ACK/NAK 대기 상태로 전이**

- **State 2: Wait for ACK or NAK**    
    - `rdt_rcv(rcvpkt)`
    - `isNAK(rcvpkt)` → 오류 시 **재전송** (`udt_send(sndpkt)`)
    - `isACK(rcvpkt)` → 정상 수신 확인, 다음 데이터 전송 준비

## Receiver FSM

- **State: Wait for call from below**    
    - `rdt_rcv(rcvpkt)`
    - `corrupt(rcvpkt)` → 오류 시 `udt_send(NAK)`
    - `notcorrupt(rcvpkt)` →  
        `extract(rcvpkt, data)` → `deliver_data(data)` → `udt_send(ACK)`

## 핵심 포인트

- 송신자는 **수신자의 상태(state)**를 직접 알 수 없음.  
    → 따라서 **ACK/NAK 신호를 통한 상태 전달**이 필요함.    
- 이런 이유로 **프로토콜(protocol)**이 필요함.


✅ rdt2.0은 **비트 오류 탐지(checksum)**와 **ACK/NAK 기반 재전송**을 **FSM 형태로 구체화한 모델**임.


# rdt2.0: Operation with No Errors

## 동작 개요

- 채널에 **비트 오류가 없을 때**의 rdt2.0 동작 과정.
- 송신자(sender)와 수신자(receiver)가 **ACK/NAK 절차 없이 정상 통신**하는 경우.    

## 송신자 (Sender)

1. `rdt_send(data)`
    - 상위 계층에서 데이터 수신.
    - `sndpkt = make_pkt(data, checksum)` → 패킷 생성.
    - `udt_send(sndpkt)` → 패킷 전송.
2. 상태 전이: **Wait for ACK or NAK**    
3. 수신자로부터 ACK 수신 시 (`isACK(rcvpkt)`)  
    → 다음 데이터 전송을 위해 **초기 상태로 복귀**.

## 수신자 (Receiver)

1. `rdt_rcv(rcvpkt)` → 패킷 수신.
2. 패킷이 손상되지 않음 (`notcorrupt(rcvpkt)`).
3. `extract(rcvpkt, data)` → 데이터 추출.
4. `deliver_data(data)` → 상위 계층에 전달.
5. `udt_send(ACK)` → 송신자에게 정상 수신 응답 전송.    

✅ 요약

- 오류가 없는 경우:  
    송신자는 데이터 전송 후 **ACK만 받고 다음 데이터 전송**.
- NAK나 재전송 과정이 필요하지 않음 → **이상적인 정상 통신 흐름**.



# rdt2.0: Corrupted Packet Scenario

## 동작 개요

- 전송 중 패킷이 **손상(corrupted)** 되었을 때의 동작 과정.
- 수신자가 오류를 감지하면 **NAK**(Negative Acknowledgement)를 송신자에게 보냄.
- 송신자는 **NAK 수신 시 재전송(retransmit)** 수행.

## 송신자 (Sender)

1. `rdt_send(data)`  
    → `sndpkt = make_pkt(data, checksum)`  
    → `udt_send(sndpkt)`    
2. 상태 전이: **Wait for ACK or NAK**
3. 수신자로부터 NAK 수신 시 (`isNAK(rcvpkt)`)  
    → 동일한 패킷을 **다시 전송 (`udt_send(sndpkt)`)**
4. ACK 수신 시 (`isACK(rcvpkt)`)  
    → 다음 데이터 전송 준비로 복귀.

## 수신자 (Receiver)

1. `rdt_rcv(rcvpkt)`    
2. 패킷이 손상됨 (`corrupt(rcvpkt)`)  
    → `udt_send(NAK)` (재전송 요청).
3. 패킷이 정상(`notcorrupt(rcvpkt)`)이면  
    → `extract(rcvpkt, data)` → `deliver_data(data)` → `udt_send(ACK)`.

✅ **요약**

- 손상된 패킷 발생 시 수신자는 **NAK**를 송신.    
- 송신자는 **NAK 수신 후 즉시 재전송**.
- rdt2.0은 **비트 오류 감지 + 재전송(Stop-and-Wait 방식)** 으로 신뢰성 확보.


# rdt2.0 Has a Fatal Flaw!

## 문제점 (ACK/NAK 손상 시)

- **ACK 또는 NAK 패킷이 손상되면**,  
    송신자는 수신 측에서 무슨 일이 일어났는지 알 수 없음.

- 무작정 재전송하면 **중복 패킷(duplicate)** 발생 가능.    

## 중복 처리 (Handling Duplicates)

- ACK/NAK가 손상된 경우 송신자는 **현재 패킷을 재전송**함.    
- 송신자는 각 패킷에 **시퀀스 번호(sequence number)**를 부여.
- 수신자는 **중복된 패킷을 폐기(discard)**하고 상위 계층으로 전달하지 않음.

## 전송 방식

**Stop and Wait**

- 송신자는 한 번에 **한 패킷만 전송**하고,  
    수신자의 응답(ACK/NAK)을 **기다린 후 다음 패킷 전송**.    

✅ 이 문제를 해결하기 위해 **rdt3.0**에서는  
시퀀스 번호를 이용한 **중복 탐지 및 재전송 제어**가 추가됨.


# rdt2.1: Sender, Handling Garbled ACK/NAKs

## 핵심 개념

- **rdt2.1**은 **ACK/NAK 손상(garbled)** 문제를 해결하기 위해  
    송신자 측에 **시퀀스 번호(sequence number)** 개념을 도입한 버전임.    
- 송신자는 **0과 1을 번갈아** 사용하여 중복 패킷을 구분함.

## 송신자 (Sender) FSM 동작

1. **Wait for call 0 from above**    
    - 상위 계층에서 데이터 수신
    - `sndpkt = make_pkt(0, data, checksum)`
    - `udt_send(sndpkt)` → 패킷 전송

2. **Wait for ACK or NAK 0**    
    - `rdt_rcv(rcvpkt)`
    - 손상(`corrupt(rcvpkt)`)되었거나 NAK 수신 시 → `udt_send(sndpkt)` (재전송)
    - 정상 ACK(`notcorrupt(rcvpkt) && isACK(rcvpkt)`)이면 → **call 1 상태로 전이**

3. **Wait for call 1 from above**    
    - `sndpkt = make_pkt(1, data, checksum)`
    - `udt_send(sndpkt)`

4. **Wait for ACK or NAK 1**    
    - ACK 손상 or NAK 수신 시 → 재전송
    - 정상 ACK 수신 시 → 다시 **call 0 상태로 전환**

## 요약

- **시퀀스 번호 0, 1 교차 사용**으로 중복 전송 문제 해결.    
- **손상된 ACK/NAK 수신 시 재전송** 수행.
- 여전히 **Stop-and-Wait 방식** 유지.

✅ rdt2.1은 rdt2.0의 치명적 결함(ACK/NAK 손상 시 모호함)을 해결하기 위한 **송신자 측 개선 모델**임.


# rdt2.1: Receiver, Handling Garbled ACK/NAKs

## 핵심 개념

- **rdt2.1**의 수신자(receiver)는 **ACK/NAK 손상(garbled)** 상황을 처리하기 위해  
    **시퀀스 번호(sequence number)**를 사용하여 올바른 패킷인지 확인함.    
- 송신자와 마찬가지로, 수신자도 **0과 1을 번갈아 처리**하며 중복 전송을 구분함.

## 수신자 (Receiver) FSM 동작

### ① **Wait for 0 from below**

- `rdt_rcv(rcvpkt)`    
    - 손상(`corrupt(rcvpkt)`) → `make_pkt(NAK, chksum)` → `udt_send(sndpkt)`
    - 정상(`notcorrupt(rcvpkt) && has_seq0(rcvpkt)`) →  
        `extract(data)` → `deliver_data(data)` →  
        `make_pkt(ACK, chksum)` → `udt_send(sndpkt)` → 다음 상태로 전이 (**Wait for 1**)
    - `notcorrupt(rcvpkt) && has_seq1(rcvpkt)` → 중복 → `send(ACK)`

### ② **Wait for 1 from below**

- `rdt_rcv(rcvpkt)`    
    - 손상(`corrupt(rcvpkt)`) → `make_pkt(NAK, chksum)` → `udt_send(sndpkt)`
    - 정상(`notcorrupt(rcvpkt) && has_seq1(rcvpkt)`) →  
        `extract(data)` → `deliver_data(data)` →  
        `make_pkt(ACK, chksum)` → `udt_send(sndpkt)` → 다음 상태로 전이 (**Wait for 0**)
    - `notcorrupt(rcvpkt) && has_seq0(rcvpkt)` → 중복 → `send(ACK)`

## 요약

- 수신자는 **시퀀스 번호를 통해 중복 패킷을 식별**하고,  
    이미 받은 패킷은 **상위 계층에 전달하지 않음**.    
- ACK/NAK가 손상되더라도 송신자와 수신자가 **패킷 순서(0/1)**를 기준으로 동기 유지 가능.


✅ rdt2.1의 수신자 FSM은 **오류 감지 + 시퀀스 번호 기반 중복 방지** 기능을 결합하여  
rdt2.0의 ACK/NAK 손상 문제를 해결함.


# rdt2.1: Discussion

## Sender

- 각 패킷에 **시퀀스 번호(seq #)**가 추가됨.
- 시퀀스 번호는 **0과 1, 두 개만(0,1)** 있으면 충분함.
    - 이유: Stop-and-Wait 방식에서는 한 번에 한 패킷만 전송되므로,  
        연속된 두 패킷을 구분하기 위해 1비트(0/1)만으로도 가능함.

- 수신한 **ACK/NAK가 손상되었는지 확인**해야 함.    
- 상태(state)가 두 배로 늘어남.
    - 이유: 송신자는 다음에 전송할 **예상 시퀀스 번호(0 또는 1)**를 기억해야 함.

## Receiver

- 수신된 패킷이 **중복인지 여부**를 검사해야 함.    
    - 상태(state)는 현재 **기대되는 시퀀스 번호(0 또는 1)**를 나타냄.
- 수신자는 자신이 보낸 마지막 **ACK/NAK가 송신자에게 정상적으로 도착했는지 알 수 없음.**


✅ **요약**

- rdt2.1에서는 송신자와 수신자 모두  
    **시퀀스 번호(0/1)**를 기반으로 상태를 관리하며,  
    **손상된 ACK/NAK 및 중복 패킷 문제를 해결**함.

# rdt2.2: A NAK-Free Protocol

## 핵심 개념

- **rdt2.1과 동일한 기능**을 수행하지만, **NAK 없이 ACK만 사용**함.
- 수신자는 **NAK 대신**, 마지막으로 **정상 수신된 패킷에 대한 ACK**를 보냄.
    - 이때, **ACK에는 명시적으로(implicitly)** 해당 패킷의 **시퀀스 번호(seq #)**가 포함되어야 함.

## 송신자 동작

- 동일한 ACK가 **중복(duplicate)**으로 수신되면,  
    송신자는 이를 **NAK와 동일하게 해석**하여 **현재 패킷을 재전송(retransmit current pkt)** 함.    

## 특징

- **rdt2.2는 rdt2.1과 동등한 신뢰성**을 가지며,  
    **NAK 없이 ACK만으로 오류 복구**를 수행.    
- **TCP(Transmission Control Protocol)** 역시 이 방식을 채택하여 **NAK-free**하게 동작함.

✅ 요약

- rdt2.2 = rdt2.1 (−NAK, +중복 ACK 사용)    
- 수신자는 **“마지막 정상 패킷 번호”를 포함한 ACK**를 전송
- 송신자는 **중복 ACK 수신 시 재전송 수행**


# rdt2.2: Sender

## 핵심 개념

- rdt2.2는 **NAK 없이 ACK만 사용하는 프로토콜**로,  
    송신자는 수신된 **ACK의 시퀀스 번호(0 또는 1)**를 기반으로 동작함.
- **중복 ACK**를 받으면 NAK와 동일하게 처리 → **현재 패킷 재전송**.

## 송신자 (Sender) FSM 동작

### ① Wait for call 0 from above

- `rdt_send(data)`  
    → `sndpkt = make_pkt(0, data, checksum)`  
    → `udt_send(sndpkt)`    
- 상태 전이 → **Wait for ACK 0**
### ② Wait for ACK 0

- `rdt_rcv(rcvpkt)`
    - 손상(`corrupt(rcvpkt)`) 또는 **잘못된 ACK (isACK(rcvpkt, 1))** → 재전송(`udt_send(sndpkt)`)
    - 정상(`notcorrupt(rcvpkt) && isACK(rcvpkt, 0)`) → **Wait for call 1** 상태로 전이
### ③ Wait for call 1 from above

- `rdt_send(data)`  
    → `sndpkt = make_pkt(1, data, checksum)`  
    → `udt_send(sndpkt)`    
- 상태 전이 → **Wait for ACK 1**
### ④ Wait for ACK 1

- `rdt_rcv(rcvpkt)`
    - 손상(`corrupt(rcvpkt)`) 또는 **잘못된 ACK (isACK(rcvpkt, 0))** → 재전송(`udt_send(sndpkt)`)
    - 정상(`notcorrupt(rcvpkt) && isACK(rcvpkt, 1)`) → **Wait for call 0** 상태로 복귀

## 요약

- **ACK만으로 신뢰성 보장** (NAK 불필요)    
- **시퀀스 번호 0/1**을 번갈아 사용
- **중복 ACK → 재전송**
- rdt2.2는 TCP의 **NAK-free 설계 원리**와 동일한 개념을 가짐.


# rdt2.2: Receiver

## 핵심 개념

- **rdt2.2**는 **NAK 없는 프로토콜**로,  
    수신자는 항상 **ACK만 전송**하여 송신자에게 상태를 알림.    
- 수신자는 패킷의 **시퀀스 번호(seq #)**를 사용해  
    올바른 패킷과 **중복 패킷을 구분**함.

## 수신자 (Receiver) FSM 동작

### ① Wait for 0 from below

- `rdt_rcv(rcvpkt)`    
    - 손상(`corrupt(rcvpkt)`) 또는 잘못된 시퀀스(`has_seq1(rcvpkt)`)  
        → `sndpkt = make_pkt(ACK, 1, chksum)` → `udt_send(sndpkt)` (이전 ACK 재전송)
    - 정상(`notcorrupt(rcvpkt) && has_seq0(rcvpkt)`)  
        → `extract(data)` → `deliver_data(data)`  
        → `sndpkt = make_pkt(ACK, 0, chksum)` → `udt_send(sndpkt)`  
        → 상태 전이 → **Wait for 1 from below**

### ② Wait for 1 from below

- `rdt_rcv(rcvpkt)`
    
    - 손상(`corrupt(rcvpkt)`) 또는 잘못된 시퀀스(`has_seq0(rcvpkt)`)  
        → `sndpkt = make_pkt(ACK, 0, chksum)` → `udt_send(sndpkt)` (이전 ACK 재전송)   
    - 정상(`notcorrupt(rcvpkt) && has_seq1(rcvpkt)`)  
        → `extract(data)` → `deliver_data(data)`  
        → `sndpkt = make_pkt(ACK, 1, chksum)` → `udt_send(sndpkt)`  
        → 상태 전이 → **Wait for 0 from below**

## 요약

- **NAK 불필요:** 수신자는 항상 ACK만 전송.    
- **중복 패킷:** 이전 ACK 재전송으로 처리.
- **시퀀스 번호(0/1)**로 순서 유지 및 중복 방지.
- 송신자는 **중복 ACK을 NAK처럼 해석**해 패킷을 재전송함.

✅ rdt2.2의 수신자는 TCP의 동작 방식과 동일하게,  
**ACK만으로 오류 복구를 수행하는 NAK-free 구조**를 구현함.


# rdt3.0: Channels with Errors and Loss

## 핵심 개념

- **새로운 채널 가정 (New Channel Assumption):**  
    이제 채널은 **패킷 손상(error)**뿐 아니라 **패킷 손실(loss)**도 발생할 수 있음.
    - 손실 대상: **데이터(data)** 또는 **ACK(확인 응답)**

- **Checksum**, **시퀀스 번호(sequence #)**, **ACK**, **재전송(retransmission)**이 도움이 되지만,  
    **이제는 이것만으로는 충분하지 않음**.    

## 주요 변화

- 송신자는 ACK이 손실될 수도 있다는 가능성을 고려해야 함.    
- 따라서 **시간 기반의 재전송 메커니즘(타이머, timer)**이 필요함.
- 즉, **ACK이 일정 시간 내에 오지 않으면 재전송** 수행.

## 인간의 비유 (Analogy)

> 사람 간 대화에서도 “들리지 않았을 때” 다시 묻는 것처럼,  
> 네트워크에서도 응답(ACK)이 없으면 **재요청(retransmission)**을 수행해야 함.


✅ **요약**

- rdt3.0은 **패킷 손실(loss)**까지 고려하는 신뢰성 전송 모델.    
- 핵심 추가 요소: **타이머(timer)**를 통한 재전송 제어.
- TCP의 기본 아이디어와 동일한 구조.


# rdt3.0: Channels with Errors and Loss

## 접근 방식 (Approach)

- 송신자는 **“적절한 시간(reasonable amount of time)”** 동안 **ACK을 대기**함.
- 해당 시간 안에 **ACK이 도착하지 않으면 재전송(retransmit)** 수행.

## 세부 동작

- 만약 패킷 또는 ACK이 **지연(delayed)** 되었지만 손실되지 않은 경우:    
    - 재전송된 패킷은 **중복(duplicate)**이지만,
    - **시퀀스 번호(sequence #)**가 이미 이를 구분하기 때문에 문제 없음.

- 수신자는 ACK 보낼 때 **어떤 패킷(시퀀스 번호)이 확인되었는지** 명시해야 함.

## 타이머 사용

- **카운트다운 타이머(countdown timer)**를 설정하여,  
    설정된 시간 내에 ACK이 도착하지 않으면 **인터럽트(interrupt)**를 발생시켜 **재전송**함. 


✅ **요약**

- rdt3.0은 **패킷 손상 + 손실 모두 처리 가능**.    
- **핵심 기법:** 타이머 기반 재전송과 시퀀스 번호로 중복 문제 해결.

# rdt3.0: Sender

## 핵심 개념

- **rdt3.0**은 손실(loss)과 오류(error)가 모두 발생할 수 있는 채널에서 **신뢰적 데이터 전송(reliable data transfer)**을 보장함.    
- 송신자(sender)는 **타이머(timer)**를 이용하여 ACK 손실 또는 지연 상황을 감지하고,  
    필요 시 **패킷 재전송(retransmission)**을 수행함.

## 송신자 동작 요약

1. **데이터 전송 요청 (rdt_send)**    
    - 송신자가 상위 계층으로부터 데이터를 전달받으면  
        → `sndpkt = make_pkt(seq, data, checksum)`  
        → `udt_send(sndpkt)` (비신뢰적 채널로 전송)  
        → **타이머 시작 (`start_timer`)**

2. **ACK 수신 (rdt_rcv)**    
    - ACK 패킷이 도착하면,
        - 손상되지 않았고(`notcorrupt`)
        - 올바른 시퀀스 번호의 ACK이면(`isACK(rcvpkt, seq)`)  
            → **타이머 중지 (`stop_timer`)**  
            → 다음 데이터 전송 단계로 이동.

3. **ACK 미수신 (Timeout 발생)**    
    - 설정된 시간 내에 ACK이 도착하지 않으면  
        → **Timeout 이벤트 발생**  
        → 같은 패킷을 다시 전송 (`udt_send(sndpkt)`)  
        → 타이머 재시작 (`start_timer`)

## FSM (유한상태기계) 구조 설명

- 두 개의 시퀀스 번호(0, 1)를 사용하여 교대로 패킷 전송.    
- 각 시퀀스 번호에 대해 다음 동작을 수행함:
    - **`Wait for call 0 (or 1) from above`**: 상위 계층에서 데이터 대기.
    - **`Wait for ACK0 (or ACK1)`**: 해당 패킷에 대한 ACK 대기.
    - ACK이 수신되면 → `stop_timer`
    - Timeout 발생 시 → 패킷 재전송 + `start_timer` 유지.

✅ **요약**

- **start_timer:** 데이터 전송 후 타이머 시작.    
- **stop_timer:** 올바른 ACK 수신 시 타이머 종료.
- **Timeout:** ACK 손실 시 자동 재전송.
- 결과적으로 rdt3.0은 **손실 및 오류 모두 복구 가능한 완전 신뢰형 프로토콜**임.


# dt3.0: Sender (송신자 동작 FSM)

## 개요

이 다이어그램은 **rdt3.0 (reliable data transfer 3.0)** 프로토콜의 **송신자(sender)** 동작을 나타냅니다.  
`rdt3.0`은 **손상된 패킷(bit errors)**과 **손실된 패킷(loss)**을 모두 처리할 수 있는 신뢰적 전송 프로토콜입니다.

## 상태(State) 설명

### 1. **Wait for call 0 from above**

- 상위 계층에서 전송 요청을 기다리는 상태.    
- 이벤트:
    - `rdt_send(data)` 발생 시  
        → `sndpkt = make_pkt(0, data, checksum)`  
        → `udt_send(sndpkt)`  
        → **타이머 시작(`start_timer`)**  
        → 다음 상태로 전이 → **Wait for ACK0**

### 2. **Wait for ACK0**

- 데이터(시퀀스 번호 0)를 보낸 뒤, 해당 패킷에 대한 ACK을 기다리는 상태.    
- 이벤트:
    - `rdt_rcv(rcvpkt)`
        - 손상되지 않고(`notcorrupt`), ACK0이면(`isACK(rcvpkt,0)`)  
            → **타이머 정지(`stop_timer`)**  
            → 다음 데이터 전송 대기 (**Wait for call 1 from above**)

    - `timeout` 발생 시  
        → 패킷 재전송(`udt_send(sndpkt)`)  
        → **타이머 재시작(`start_timer`)**        

### 3. **Wait for call 1 from above**

- ACK0이 도착한 뒤, 시퀀스 번호 1의 새 데이터를 전송하기 위해 대기하는 상태.    
- 이벤트:
    - `rdt_send(data)`  
        → `sndpkt = make_pkt(1, data, checksum)`  
        → `udt_send(sndpkt)`  
        → **타이머 시작(`start_timer`)**  
        → 다음 상태로 전이 → **Wait for ACK1**

### 4. **Wait for ACK1**

- 데이터(시퀀스 번호 1)를 보낸 뒤, 해당 ACK을 기다리는 상태.    
- 이벤트:
    - `rdt_rcv(rcvpkt)`
        - 손상되지 않고(`notcorrupt`), ACK1이면(`isACK(rcvpkt,1)`)  
            → **타이머 정지(`stop_timer`)**  
            → 다음 데이터 전송 대기 (**Wait for call 0 from above**)

    - `timeout` 발생 시  
        → 패킷 재전송(`udt_send(sndpkt)`)  
        → **타이머 재시작(`start_timer`)**        

## 타이머 역할 요약

| 이벤트          | 동작                              |
| ------------ | ------------------------------- |
| 데이터 전송 직후    | **start_timer**                 |
| 올바른 ACK 수신   | **stop_timer**                  |
| ACK 손실 또는 지연 | **timeout → 재전송 + start_timer** |

✅ **정리**

- `rdt3.0`은 **타이머 기반 재전송 기법**으로 손실을 복구함.    
- **시퀀스 번호 0과 1을 교대로 사용**하여 중복 패킷을 구분.
- `ACK`, `timeout`, `retransmission`을 이용하여 **완전한 신뢰성 보장**.


# rdt3.0 in Action

## (a) **No Loss** – 손실 없는 경우

### 송신자(sender) ↔ 수신자(receiver)의 정상적인 데이터 흐름

1. **송신자:** `send pkt0` → **수신자:** `rcv pkt0`, `send ack0`
2. **송신자:** `rcv ack0`, `send pkt1` → **수신자:** `rcv pkt1`, `send ack1`
3. **송신자:** `rcv ack1`, `send pkt0` → **수신자:** `rcv pkt0`, `send ack0`

- 송신자는 **pkt0, pkt1**을 번갈아가며 전송함.
- 각 데이터에 대해 ACK을 정확히 수신하면 다음 패킷 전송.
- 모든 데이터가 정상적으로 교환되므로 **타임아웃 발생 없음**.

## (b) **Packet Loss** – 패킷 손실 발생 시

1. **송신자:** `send pkt0` → **수신자:** `rcv pkt0`, `send ack0`    
2. **송신자:** `rcv ack0`, `send pkt1`  
    → **패킷 손실 발생 (pkt1 loss)**
3. 송신자는 ACK을 기다리지만 도착하지 않음 → **timeout 발생**    
4. **Timeout 후 재전송:** `resend pkt1`  
    → **수신자:** `rcv pkt1`, `send ack1`
5. **송신자:** `rcv ack1`, `send pkt0` → **정상 복구**

## 요약 (Summary)

| 상황              | 동작                    | 결과                  |
| --------------- | --------------------- | ------------------- |
| (a) No Loss     | 모든 패킷과 ACK이 정상 전달     | 연속적인 전송             |
| (b) Packet Loss | 송신자가 타이머로 손실 감지 → 재전송 | 정상 복구 (중복 없는 신뢰 전송) |

✅ **핵심 포인트:**

- **타이머(timeout)** 기반으로 손실된 패킷을 복구함.    
- **시퀀스 번호(seq #)** 덕분에 중복 패킷을 구분할 수 있음.
- 결과적으로 rdt3.0은 **손상 + 손실**이 모두 존재하는 환경에서도 **완전한 신뢰적 전송(reliable transfer)**을 보장함.


# rdt3.0 in Action (continued)

## (c) **ACK Loss** – 수신자의 ACK 손실

1. **송신자(sender):**
    - `send pkt0` → **수신자(receiver):** `rcv pkt0`, `send ack0`
    - `rcv ack0`, `send pkt1`
    - **ACK1 손실 발생 (ack1 loss)**
2. **송신자:** ACK1이 도착하지 않음 → **timeout 발생**  
    → **resend pkt1**    
3. **수신자:** `rcv pkt1` → 이미 받은 데이터임을 인식 (**detect duplicate**)  
    → **중복 ACK(ack1)** 재전송
4. **송신자:** `rcv ack1` → 다음 패킷(pkt0) 전송

✅ **핵심 요약:**

- ACK이 손실되어도 송신자는 타임아웃 후 재전송함.
- 수신자는 중복 패킷을 감지하고 ACK만 다시 보냄.
- 데이터가 중복으로 전달되지 않음 (**신뢰성 유지**).

## (d) **Premature Timeout / Delayed ACK** – 조기 타임아웃 또는 ACK 지연

1. **송신자:**    
    - `send pkt0` → **수신자:** `rcv pkt0`, `send ack0`
    - `rcv ack0`, `send pkt1`

2. **ACK1이 지연되어 도착하지 않음** → 송신자 **timeout 발생**  
    → `resend pkt1` (중복 전송)    

3. **수신자:**
    - `rcv pkt1` → 중복 패킷으로 판단 (**detect duplicate**)
    - 다시 `send ack1`

4. **이후 지연된 ack1이 도착**  
    → 송신자는 이를 **무시(ignore)** (이미 처리 완료).    

✅ **핵심 요약:**

- ACK이 늦게 도착하거나 조기 timeout이 발생해도,  
    **시퀀스 번호(seq #)**와 **중복 감지** 덕분에 혼선 없이 정상 복구됨.    
- rdt3.0은 **손실 + 지연 + 중복 상황에서도 안정적 동작**을 보장함.

### 📘 전체 요약

| 시나리오                                | 문제 유형                | 해결 방법             | 결과          |
| ----------------------------------- | -------------------- | ----------------- | ----------- |
| (c) ACK Loss                        | ACK 손실               | 타임아웃 후 패킷 재전송     | 중복 ACK으로 복구 |
| (d) Premature Timeout / Delayed ACK | ACK 지연 또는 조기 timeout | 중복 감지로 올바른 ACK 처리 | 정상 전송 유지    |

✅ **결론:**  
`rdt3.0`은 타임아웃, 재전송, 시퀀스 번호, 중복 ACK 처리로  
**모든 오류(손상, 손실, 지연)를 감지하고 복구할 수 있는 완전한 신뢰적 전송 프로토콜**입니다.


# Reliable Data Transfer Protocol: 1.0 ~ 3.0 요약

| **rdt version** | **Issues (문제점)**                          | **Solutions (해결 방법)**                                                                                         |
| --------------- | ----------------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| **1.0**         | 완벽히 신뢰할 수 있는 채널 (오류 없음)                   | 별도의 오류 처리 불필요                                                                                                 |
| **2.0**         | 채널에 비트 오류 발생 (패킷의 비트가 뒤집힘)                | **ACK**와 **NAK** 사용  <br>수신자가 NAK을 보내면 송신자는 해당 패킷 재전송                                                         |
| **2.1**         | 비트 오류 있는 채널에서 **ACK/NAK 손상 시** 문제 발생      | 송신자는 ACK/NAK이 손상되면 **현재 패킷 재전송**  <br>송신자는 각 패킷에 **시퀀스 번호(seq #)** 추가  <br>수신자는 **중복 패킷(duplicate)**을 감지 후 폐기 |
| **2.2**         | 비트 오류 존재, **NAK 없는(Nak-free) 프로토콜** 구현 필요 | NAK 대신 수신자가 **가장 최근에 제대로 받은 패킷의 ACK** 전송  <br>중복 ACK을 통해 송신자가 재전송 여부 판단                                       |
| **3.0**         | 오류와 **패킷 손실(loss)** 모두 발생 가능              | 송신자는 **일정 시간(“reasonable” time)** 동안 ACK을 기다림  <br>ACK이 오지 않으면 **timeout 후 재전송** 수행                           |

## 핵심 요약

- **rdt 1.0 → 3.0**은 점점 더 현실적인 네트워크 환경을 반영.    
- **비트 오류**, **ACK/NAK 손상**, **패킷 손실** 등 다양한 문제에 대해 단계별로 복구 메커니즘 추가.
- 최종적으로 **rdt3.0**은  
    → **오류 검출(Checksum)** + **시퀀스 번호 관리** + **타이머 기반 재전송**을 통해  
    완전한 **신뢰적 데이터 전송 (Reliable Data Transfer)** 을 구현함.


### 📘 Performance of rdt3.0 (Stop-and-Wait)

**Uₛₑₙdₑᵣ : utilization (이용률)**  
→ 송신자가 데이터를 전송하느라 **바쁘게 일하는 시간의 비율**을 의미합니다.

**예시 조건**

- 전송속도(link rate): 1 Gbps
- 전파 지연(propagation delay): 15 ms
- 패킷 크기: 8000 bits

**패킷을 전송 채널에 넣는 데 걸리는 시간**

Dtrans=LR=8000 bits109 bits/sec=8 microseconds
즉, 송신자는 8 μs 동안 패킷을 전송하고, 그 후에는 ACK(승인 응답)를 기다려야 하므로 실제 사용 효율(이용률)은 전송 시간 대비 대기 시간의 비율에 의해 크게 낮아집니다.

**간단한 설명**  
이 슬라이드는 **rdt3.0 (Stop-and-Wait 프로토콜)**의 효율을 분석하는 예시입니다.  
송신자는 한 번에 하나의 패킷만 전송하고, 수신자의 응답을 기다린 후 다음 패킷을 보냅니다. 따라서 전송 속도는 매우 높더라도 지연이 크면 대부분의 시간이 **대기 시간**으로 낭비되어 **이용률이 낮아지는 문제**가 발생합니다.


### 📘 rdt3.0: Stop-and-Wait Operation (정지-대기 방식 동작)

**송신자 이용률 (Uₛₑₙdₑᵣ)**

U_sender=(L/R)/RTT+L/R = 0.008/30.008=0.00027

- **L/R** : 패킷 전송 시간 (8 μs)
- **RTT (Round Trip Time)** : 왕복 지연 시간 (30 ms = 0.03 s)

따라서 송신자는 전체 시간 중 단지 **0.027%**만 데이터를 실제로 전송하고, 나머지 **99.973%**는 ACK 응답을 기다리는 데 소요됩니다.

### 🔹 결과 해석

- **rdt 3.0의 성능은 매우 낮습니다.**  
    (rdt 3.0 protocol performance stinks!)
 
- **프로토콜의 구조적 한계로 인해**  
    네트워크 인프라(채널)의 실제 성능을 활용하지 못합니다.  
    즉, 링크 속도가 아무리 높아도 지연시간이 길면 전송 효율이 급격히 떨어집니다.    

### 🧩 간단 요약

Stop-and-Wait 방식에서는 송신자가 **패킷 하나를 보내고 ACK를 기다린 후** 다음 패킷을 전송합니다.  
이 때문에 RTT가 큰 환경(예: 장거리 통신)에서는 송신 대기 시간이 대부분을 차지하여 **이용률이 극도로 낮아지는 비효율적 구조**입니다.


### 📘 rdt3.0: Pipelined Protocols Operation (파이프라인 프로토콜 동작)

**Pipelining**  
→ 송신자가 **여러 개의 패킷을 동시에 전송(in-flight)** 하고,  
아직 **ACK(확인 응답)** 을 받지 않은 상태에서도 다음 패킷을 보낼 수 있도록 허용하는 방식입니다.

**특징**

- **Sequence number의 범위 증가 필요**  
    → 여러 패킷이 동시에 전송되므로, 각 패킷을 구별하기 위해 더 넓은 시퀀스 번호 공간이 필요합니다.

- **Buffering 필요**  
    → 송신자(sender)와 수신자(receiver) 모두 임시 저장 공간(buffer)이 필요합니다.  
    → 송신자는 아직 ACK가 오지 않은 패킷을 보관해야 하며, 수신자는 순서가 뒤섞여 도착한 패킷을 임시 저장해야 합니다.


**비교 설명 (그림 해석)**

- **(a)** Stop-and-Wait 방식: 한 번에 한 개의 패킷만 전송하고 ACK를 기다림.    
- **(b)** Pipelined 방식: 여러 패킷을 연속적으로 전송하면서 ACK를 동시에 받음.

이로 인해 **채널의 유휴 시간(idle time)** 이 크게 줄어들고,  
**전송 효율(throughput)** 이 현저히 향상됩니다.

**요약**  
Stop-and-Wait은 단순하지만 매우 비효율적입니다.  
Pipelining은 복잡하지만, 대기 시간을 숨기면서 **링크의 성능을 최대한 활용**할 수 있는 핵심 기술입니다.


### 📘 Pipelining: Increased Utilization (파이프라이닝을 통한 이용률 향상)

**개념 요약**  
Stop-and-Wait 방식에서는 송신자가 매번 ACK를 기다리며 멈추지만,  
Pipelining에서는 여러 패킷을 연속적으로 전송함으로써 **대기 시간을 숨기고 전송 효율을 높입니다.**

**식 유도**

U_sender=(3L/R)/RTT+L/R​ =0.0024/30.008=0.00081

→ 3개의 패킷을 동시에 전송(3-packet pipelining)하면,  
이용률이 기존(0.00027)에 비해 **3배 증가**합니다.

**그래프 해석**

- 송신자는 첫 번째 패킷을 보낸 직후 두 번째, 세 번째 패킷을 연속적으로 전송함.
- 수신자는 각 패킷을 받는 즉시 ACK를 반환함. 
- 결과적으로 **RTT(왕복 지연 시간)** 동안 채널이 **계속 사용**되므로 idle(유휴) 시간이 크게 감소함.

**핵심 요점**

- Stop-and-Wait: 한 번에 1개의 패킷만 전송 → 이용률 매우 낮음    
- 3-Packet Pipelining: 한 번에 3개의 패킷 전송 → **이용률 3배 향상**
- 따라서 파이프라이닝은 **고지연 환경에서도 효율적인 전송**을 가능하게 하는 핵심 기법입니다.



### 📘 Go-Back-N: Sender (송신자 측 동작)

**개념 요약**  
Go-Back-N은 **파이프라인 전송(pipelined transmission)** 기법 중 하나로,  
송신자가 한 번에 여러 패킷(최대 N개)을 전송할 수 있도록 하는 **슬라이딩 윈도우(sliding window)** 기반 프로토콜입니다.

**🔹 동작 구조**

- **Sender window (송신 윈도우)**  
    최대 **N개의 연속된 패킷**을 전송할 수 있으며, ACK를 받지 않은 상태에서도 다음 패킷을 보낼 수 있습니다.
    - **already ACK’ed (초록색)** : 수신 측에서 이미 확인된 패킷
    - **sent, not yet ACK’ed (노란색)** : 전송되었으나 아직 확인(ACK)되지 않은 패킷
    - **usable, not yet sent (파란색)** : 전송 가능한 다음 패킷
    - **not usable (흰색)** : 윈도우 밖에 있어 아직 전송 불가능한 패킷

**🔹 주요 메커니즘**

1. **누적 확인 응답 (Cumulative ACK)**    
    - `ACK(n)`은 **순서 번호 n까지의 모든 패킷이 정상 수신되었음**을 의미합니다.
    - 송신자는 `ACK(n)`을 받으면 **윈도우를 앞으로 이동**시켜 다음 패킷 전송을 시작합니다.

2. **타이머 (Timer)**    
    - 송신자는 **가장 오래된 미확인 패킷(가장 앞쪽 패킷)** 에 대해 타이머를 설정합니다.

3. **시간 초과 시 (Timeout)**    
    - 만약 패킷 `n`이 ACK되지 않고 타임아웃이 발생하면,  
        **패킷 n 및 그 이후의 모든 패킷을 재전송**합니다.  
        (즉, “Go-Back”하여 다시 전송)


**🔹 핵심 요약**

- Go-Back-N은 Stop-and-Wait보다 훨씬 효율적입니다.    
- 다만, **한 패킷 손실로 인해 여러 패킷을 다시 보내야 하므로** 효율이 완벽하지는 않습니다.
- 이 프로토콜은 **단순성과 성능 간의 절충점**으로 TCP의 기본 개념과 유사한 구조를 가집니다.


### 📘 Go-Back-N: Sender Extended FSM (송신자 확장 유한상태머신)

**개요**  
이 다이어그램은 Go-Back-N 프로토콜의 **송신자(sender)** 동작을 상태 기계(Finite State Machine, FSM)로 표현한 것입니다.  
송신자는 **하나의 상태(Wait)** 내에서 여러 이벤트(데이터 전송, ACK 수신, 타임아웃 등)에 따라  
다양한 동작을 수행합니다.

### 🔹 상태 및 변수 정의

- **base** : 현재 윈도우에서 **가장 오래된 전송된 패킷의 시퀀스 번호**
- **nextseqnum** : **다음에 전송할 패킷의 시퀀스 번호**
- **N** : 윈도우 크기(window size)
- **sndpkt[]** : 송신 측에서 보관 중인 전송 패킷 배열

### 🔹 주요 동작

#### 🟢 1. 데이터 전송 요청 (`rdt_send(data)`)

`if (nextseqnum < base + N)     sndpkt[nextseqnum] = make_pkt(nextseqnum, data, chksum)     udt_send(sndpkt[nextseqnum])     if (base == nextseqnum)         start_timer()     nextseqnum++ else     refuse_data(data)`

- 윈도우 안에 여유가 있으면 새 패킷을 만들어 전송.    
- 만약 **현재 전송 중인 패킷이 없는 경우(base == nextseqnum)** → 타이머 시작.
- 윈도우가 꽉 찼을 경우(`nextseqnum >= base + N`) → 전송 요청 거부(refuse_data).

#### 🔵 2. ACK 수신 (`rdt_rcv(rcvpkt) && notcorrupt(rcvpkt)`)

`base = getacknum(rcvpkt) + 1 if (base == nextseqnum)     stop_timer() else     start_timer()`

- 정상적인 ACK 수신 시, base를 이동하여 윈도우를 앞으로 전진.    
- 모든 패킷이 확인되면 타이머 정지.
- 아직 미확인 패킷이 남아 있으면 타이머 재시작.

#### 🔴 3. 타임아웃 발생 (`timeout`)

`start_timer() udt_send(sndpkt[base]) udt_send(sndpkt[base+1]) ... udt_send(sndpkt[nextseqnum-1])`

- 가장 오래된 패킷부터 윈도우 내 모든 미확인 패킷을 **재전송(go back)**.    
- 이후 타이머를 다시 시작.

#### ⚫ 4. 손상된 ACK 수신 (`rdt_rcv(rcvpkt) && corrupt(rcvpkt)`)

- 손상된 ACK는 무시하고 **상태 유지(Wait)**.    
### 🔹 요약 정리

| 이벤트              | 동작 요약                    |
| ---------------- | ------------------------ |
| `rdt_send(data)` | 윈도우 내 여유가 있으면 전송, 없으면 거부 |
| `rdt_rcv(ACK)`   | base 갱신, 윈도우 이동          |
| `timeout`        | base부터 모든 패킷 재전송         |
| `corrupt(ACK)`   | 무시, 상태 유지                |


**핵심 개념**

- 송신자는 **하나의 타이머만 유지**하며,  
    **가장 오래된 패킷(base)** 기준으로 재전송을 수행합니다.    
- ACK가 누락되거나 손상되면 **윈도우 내 모든 패킷을 다시 보내는 구조**이므로,  
    효율은 떨어지지만 **단순하고 안정적인 신뢰성 보장**이 가능합니다.


### 📘 Go-Back-N: Receiver (수신자 측 동작)

**개념 요약**  
Go-Back-N 프로토콜에서 **수신자(receiver)** 는 매우 단순한 동작을 수행합니다.  
수신자는 오직 **순서대로(in-order)** 도착한 패킷까지만 ACK를 전송하며,  
순서가 어긋난(out-of-order) 패킷은 기본적으로 **저장하지 않거나 무시**합니다.

### 🔹 주요 동작 원리

1. **ACK-Only 정책**    
    - 수신자는 **지금까지 순서대로 잘 수신된 패킷 중 가장 큰 시퀀스 번호**(in-order seq #)에 대해 ACK를 전송합니다.
    - 예: `ACK(n)` → “0부터 n까지의 모든 패킷을 정상적으로 받았다”는 의미
    - 동일한 ACK를 여러 번 보낼 수도 있음 (중복 ACK 발생 가능)
    - 수신자는 단 하나의 변수만 기억하면 됨 → **`rcv_base` (다음에 기대되는 패킷 번호)**

2. **Out-of-order 패킷 수신 시**
    - 순서가 맞지 않는 패킷이 도착하면 다음 두 가지 처리 방법 중 하나를 선택할 수 있음: 
        - **Discard (버림)** : 버퍼링하지 않고 폐기 (Go-Back-N 기본 방식)
        - **Buffer (임시저장)** : 나중에 사용할 수 있도록 저장 (일부 구현체에서 선택적 사용)

    - 어떤 방식을 택하든, 수신자는 항상 **가장 높은 in-order 패킷 번호**에 대한 ACK를 다시 보냄. 

### 🔹 시퀀스 번호 공간에서의 수신자 관점

| 색상              | 의미                               |
| --------------- | -------------------------------- |
| 🟩 **초록색**      | 이미 수신 및 ACK 전송 완료된 패킷            |
| 🟥 **분홍색**      | 순서가 어긋나 수신되었지만 아직 ACK되지 않음       |
| ⬜ **흰색**        | 아직 수신되지 않음                       |
| 🔹 **rcv_base** | 다음으로 기대하는 시퀀스 번호 (in-order의 기준점) |

### 🔹 핵심 요약

- 수신자는 **가장 단순한 구조**를 가집니다.    
- 항상 “지금까지 순서대로 받은 마지막 패킷 번호”만 기억하고, 그 번호에 대한 **누적 ACK(cumulative ACK)** 를 보냅니다.
- 순서가 어긋난 패킷은 무시되므로, 송신자는 해당 지점부터 **모든 이후 패킷을 다시 전송(go back)** 해야 합니다.

→ **장점:** 구현이 단순하고 처리량 예측이 쉬움  
→ **단점:** 하나의 손실로 인해 여러 패킷이 재전송되어 비효율 발생


### 📘 Go-Back-N: Receiver Extended FSM (수신자 확장 유한상태머신)

**개요**  
이 다이어그램은 Go-Back-N 프로토콜에서 **수신자(receiver)** 의 동작을 상태 기계(Finite State Machine, FSM) 형태로 표현한 것입니다.  
수신자는 매우 단순하게 하나의 상태(`Wait`)만을 유지하면서,  
수신 패킷의 **순서 번호(expectedseqnum)** 를 기준으로 동작합니다.

### 🔹 초기 설정 (Initialization)

`expectedseqnum = 1 sndpkt = make_pkt(0, ACK, chksum)`

- 수신자가 처음 시작할 때, 다음에 기대하는 패킷 번호는 `1`.
- `sndpkt`는 가장 최근에 전송한 ACK 패킷을 의미하며,  
    만약 순서가 어긋난 패킷이 오면 이 `sndpkt`를 다시 전송합니다.

### 🔹 정상적인 수신 (In-order packet 수신)

`rdt_rcv(rcvpkt) && notcorrupt(rcvpkt) && hasseqnum(rcvpkt, expectedseqnum)`

**조건:**

- 패킷이 손상되지 않았고(`notcorrupt`),    
- 예상한 시퀀스 번호(`expectedseqnum`)를 가진 경우

**동작:**

`extract(rcvpkt, data) deliver_data(data) sndpkt = make_pkt(expectedseqnum, ACK, chksum) udt_send(sndpkt) expectedseqnum++`

- 패킷의 데이터를 추출(`extract`)하고, 상위 계층에 전달(`deliver_data`).
- 새로운 ACK 패킷 생성 및 전송.
- 다음에 기대할 시퀀스 번호 증가.

### 🔹 순서가 어긋나거나 손상된 패킷 수신 (Out-of-order or corrupted)

`any other event udt_send(sndpkt)`

- 패킷이 손상되었거나, 예상된 번호가 아니면 새로 처리하지 않음.    
- **이전에 보냈던 가장 최근의 ACK(sndpkt)** 를 다시 전송함.
- 즉, 수신자는 항상 “가장 최근에 제대로 받은 패킷까지”를 재확인(ACK 재전송).

### 🔹 핵심 요약

| 이벤트            | 동작                               |
| -------------- | -------------------------------- |
| 정상적인 순서의 패킷 수신 | 데이터 전달 + 새로운 ACK 전송 + 기대 번호 증가   |
| 손상 또는 순서 불일치   | 이전 ACK 재전송                       |
| 초기 상태          | expectedseqnum = 1, 기본 ACK 패킷 준비 |


**핵심 포인트**

- 수신자는 **하나의 상태(Wait)** 만 유지하며, 매우 단순한 구조를 가짐.    
- 항상 **가장 최근의 in-order 패킷까지의 ACK** 를 유지함.
- 순서가 어긋난 패킷을 버림으로써, 송신자는 해당 지점부터 **모든 패킷을 재전송**하게 됨.  
    → 이는 Go-Back-N의 **누적 확인(cumulative ACK)** 메커니즘의 핵심입니다.


### 📘 Go-Back-N in Action (동작 예시)

**개요**  
이 슬라이드는 **Go-Back-N 프로토콜의 실제 동작 과정**을 시각적으로 나타낸 것입니다.  
송신자는 **윈도우 크기 N=4**로 설정되어 있으며, 한 번에 4개의 패킷을 연속적으로 전송합니다.

### 🔹 동작 순서 요약

#### ① 초기 전송

`송신자(sender):  pkt0, pkt1, pkt2, pkt3 전송 수신자(receiver):   - pkt0 수신 → ack0 전송 - pkt1 수신 → ack1 전송 - pkt2 손실(loss 발생)`

→ 패킷 2가 손실되었으므로, 이후 도착하는 pkt3은 **순서가 어긋난 패킷**으로 처리됨.  
→ 수신자는 pkt3, pkt4, pkt5 모두 버리고 **ack1(중복 ACK)** 을 계속 보냄.

#### ② 중복 ACK 처리

`송신자: 중복된 ack1을 여러 번 받지만 무시(ignore duplicate ACK)`

→ Go-Back-N에서는 **Selective Repeat처럼 특정 패킷만 재전송하지 않고**,  
**타임아웃**이 발생할 때까지 기다립니다.

#### ③ 타임아웃 발생

`pkt2 timeout 발생 ⏰`

→ 송신자는 pkt2를 포함해 **윈도우 내의 모든 패킷(pkt2~pkt5)을 재전송**합니다.  
→ 즉, “Go Back”하여 재전송하는 구조입니다.

#### ④ 재전송 및 정상 수신

`수신자:  - pkt2 수신 → deliver, ack2 - pkt3 수신 → deliver, ack3 - pkt4 수신 → deliver, ack4 - pkt5 수신 → deliver, ack5`

→ 이후 모든 패킷이 정상적으로 순서대로 수신 및 전달됨.

### 🔹 핵심 개념 정리

| 개념                          | 설명                                  |
| --------------------------- | ----------------------------------- |
| **윈도우 크기 (N)**              | 송신자가 한 번에 전송할 수 있는 패킷 수 (여기서는 4개)   |
| **누적 ACK (Cumulative ACK)** | 마지막으로 순서대로 받은 패킷까지 ACK              |
| **패킷 손실 시 동작**              | 손실 발생 → 타임아웃 발생 시 해당 지점부터 모든 패킷 재전송 |
| **중복 ACK**                  | 무시됨 (타임아웃만이 재전송 트리거 역할 수행)          |

### 🔹 결론

- Go-Back-N은 **순서 불일치 발생 시, 버퍼링 없이 모든 이후 패킷을 폐기**하므로 단순하지만 비효율적입니다.    
- 그러나 **연속된 패킷을 전송할 수 있는 윈도우 구조**를 통해 Stop-and-Wait보다 훨씬 높은 효율을 가집니다.
- 핵심은 “**하나의 손실이 여러 패킷 재전송을 유발한다**”는 점입니다.


### 📘 Selective Repeat (선택적 재전송)

**개요**  
Selective Repeat(선택적 반복) 프로토콜은 **Go-Back-N의 비효율성을 개선한 신뢰적 데이터 전송 방식**입니다.  
핵심 개념은 **패킷 단위로 개별적인 확인(ACK)과 재전송**을 수행한다는 점입니다.

### 🔹 1. 수신자(Receiver)의 동작

- **각 패킷별로(individually)** ACK을 보냄  
    → 모든 정상적으로 수신된 패킷에 대해 개별 ACK 전송

- **버퍼링(buffering)**  
    → 순서가 어긋난(out-of-order) 패킷이라도 임시 저장 후,  
    누락된 패킷이 도착하면 **상위 계층으로 순서대로(in-order)** 전달

- Go-Back-N과 달리 **패킷을 버리지 않음**


### 🔹 2. 송신자(Sender)의 동작

- 각 패킷마다 **개별 타이머(timer)** 를 유지    
- 특정 패킷이 ACK되지 않으면 **그 패킷만 재전송**  
    → Go-Back-N처럼 전체 윈도우를 다시 보내지 않음

### 🔹 3. 송신 윈도우(Sender Window)

- 크기: **N개의 연속된 시퀀스 번호(seq #)**    
- 송신자는 N개 미만의 패킷만 전송 가능
- ACK되지 않은(unACKed) 패킷의 수를 제한함으로써  
    네트워크 혼잡을 방지

### 🔹 핵심 비교 요약

| 구분      | Go-Back-N           | Selective Repeat        |
| ------- | ------------------- | ----------------------- |
| ACK 방식  | 누적 ACK (Cumulative) | 개별 ACK (Individual)     |
| 패킷 손실 시 | 손실 이후 모든 패킷 재전송     | 손실된 패킷만 재전송             |
| 수신자 처리  | 순서 어긋난 패킷 버림        | 순서 어긋난 패킷 저장(Buffering) |
| 효율성     | 낮음 (불필요한 재전송 발생)    | 높음 (정확한 재전송만 수행)        |

### 🧩 결론

Selective Repeat은 **데이터 흐름 제어와 신뢰성 보장 모두를 향상**시키는 프로토콜입니다.  
복잡도는 증가하지만, **불필요한 재전송을 최소화**하고 **채널 효율을 극대화**합니다.



### 📘 Selective Repeat: Sender and Receiver Windows (송신자 및 수신자 윈도우)

**개요**  
Selective Repeat(선택적 반복) 프로토콜에서는 송신자와 수신자가 **서로 독립적인 윈도우(window)** 를 유지합니다.  
각 윈도우는 **동시에 여러 패킷을 전송 및 수신**할 수 있게 하며,  
패킷 손실이나 순서 불일치가 있어도 효율적인 전송을 가능하게 합니다.

### 🔹 (a) 송신자 윈도우 (Sender View)

| 구분                          | 설명                        |
| --------------------------- | ------------------------- |
| 🟩 **already ACK’ed**       | 수신자로부터 ACK를 받은 패킷 (전송 완료) |
| 🟨 **sent, not yet ACK’ed** | 이미 전송되었으나 ACK를 받지 못한 패킷   |
| 🟦 **usable, not yet sent** | 전송 가능한 패킷 (윈도우 내에 포함됨)    |
| ⬜ **not usable**            | 아직 전송할 수 없는 패킷 (윈도우 밖)    |

**주요 포인트**

- 송신자는 `send_base`부터 `nextseqnum`까지의 **윈도우 크기 N** 내에서만 패킷을 전송할 수 있습니다.    
- 각 패킷은 **개별 타이머**를 가지며, ACK가 오지 않으면 해당 패킷만 재전송합니다.
- 이미 ACK된 패킷은 윈도우가 앞으로 이동하면서 제외됩니다.

### 🔹 (b) 수신자 윈도우 (Receiver View)

| 구분                                | 설명                               |
| --------------------------------- | -------------------------------- |
| 🟩 **received and ACK’ed**        | 순서대로 도착해 이미 ACK된 패킷              |
| 🟥 **out of order (buffered)**    | 순서가 어긋났지만 버퍼에 저장된 패킷             |
| ⚫ **expected, not yet received**  | 수신자가 다음으로 기대하는 패킷 (`rcv_base`)   |
| 🟦 **acceptable (within window)** | 아직 도착하지 않았지만 윈도우 범위 내의 허용 가능한 패킷 |
| ⬜ **not usable**                  | 윈도우 범위를 벗어난 패킷 (수신 거부)           |

**주요 포인트**

- 수신자는 순서가 어긋난 패킷도 **임시 저장(buffering)** 하며,  
    누락된 패킷이 도착하면 상위 계층에 순서대로 전달합니다.    
- `rcv_base`는 다음으로 수신되어야 할 **가장 작은 시퀀스 번호**를 가리킵니다.

### 🔹 핵심 비교 요약

| 항목     | 송신자(Sender)                       | 수신자(Receiver)                   |
| ------ | --------------------------------- | ------------------------------- |
| 윈도우 기준 | `send_base` ~ `send_base + N - 1` | `rcv_base` ~ `rcv_base + N - 1` |
| 역할     | 여러 패킷 동시 전송 및 개별 재전송              | 순서 불일치 패킷 버퍼링 및 개별 ACK 전송       |
| 윈도우 이동 | ACK 수신 시 이동                       | 순서대로 패킷 수신 시 이동                 |

### 🧩 결론

Selective Repeat은 **송신자와 수신자가 각자 윈도우를 독립적으로 관리**함으로써,  
패킷 손실이나 순서 불일치 상황에서도 **필요한 패킷만 재전송**하고  
**전체 전송 효율을 극대화**하는 구조를 제공합니다.


### 📘 Selective Repeat: Sender and Receiver (송신자와 수신자 동작 비교)

**개요**  
Selective Repeat 프로토콜에서 송신자(sender)와 수신자(receiver)는  
각각 **개별 패킷 단위로 ACK 및 재전송을 수행**합니다.  
이는 Go-Back-N과 달리 **손실된 패킷만 선택적으로 재전송**함으로써 효율을 높이는 구조입니다.

### 🔹 송신자(Sender)

#### ✅ **data from above (상위 계층에서 데이터가 도착했을 때)**

- 만약 다음 전송 가능한 시퀀스 번호가 **송신 윈도우 안에 있다면**, 패킷 전송 수행  
    (즉, 아직 윈도우가 꽉 차지 않았다면 전송 가능)

#### ⏰ **timeout(n)**

- 특정 패킷 **n**의 타이머가 만료되면  
    → 해당 **패킷 n만 재전송**하고 타이머를 재시작

#### 📩 **ACK(n) 수신 시**

- `ACK(n)`이 현재 윈도우 범위 `[sendbase, sendbase + N]` 내에 있다면  
    → 해당 패킷을 **정상 수신(ACKed)** 상태로 표시

- 만약 **n이 가장 작은 미확인 패킷**이라면  
    → **send_base**를 앞으로 이동하여 윈도우를 갱신    

### 🔹 수신자(Receiver)

#### 📨 **packet n 수신 시 (n ∈ [rcvbase, rcvbase+N-1])**

- `ACK(n)` 전송    
- 순서가 맞지 않으면 (**out-of-order**) → 버퍼에 저장
- 순서가 맞으면 (**in-order**) → 데이터 전달 후  
    버퍼 내 저장된 순서 맞는 패킷들도 함께 전달  
    → 그 다음 미수신 패킷으로 윈도우 이동

#### 🔁 **packet n 수신 시 (n ∈ [rcvbase−N, rcvbase−1])**

- 이미 수신되어 ACK된 패킷일 경우에도 `ACK(n)`을 다시 전송  
    (중복 ACK, 재확인용)

#### 🚫 **그 외 (otherwise)**

- 윈도우 밖의 패킷은 무시 (ignore)

### 🔹 핵심 요약 비교

| 항목     | 송신자(Sender)             | 수신자(Receiver)               |
| ------ | ----------------------- | --------------------------- |
| 전송 조건  | 윈도우 내 다음 시퀀스 번호 가능 시 전송 | 허용 윈도우 내 패킷만 수용             |
| ACK 처리 | 개별 ACK에 따라 윈도우 이동       | 각 패킷별로 개별 ACK 전송            |
| 타임아웃   | 개별 패킷 단위 재전송            | 재전송 요청 불필요 (ACK 반복 전송)      |
| 버퍼링    | 없음                      | 순서 불일치(out-of-order) 패킷 버퍼링 |

### 🧩 결론

Selective Repeat은 **송신자와 수신자 모두 개별적으로 윈도우를 관리**하며,  
손실된 패킷만 효율적으로 재전송함으로써  
**Go-Back-N보다 훨씬 높은 효율과 네트워크 활용도를 제공**합니다.



### 📘 Selective Repeat in Action (동작 예시)

**개요**  
이 슬라이드는 **Selective Repeat (SR) 프로토콜**의 실제 동작 과정을  
패킷 손실(loss) 상황을 포함하여 시각적으로 보여줍니다.  
윈도우 크기(N)는 4이며, **각 패킷을 개별적으로 ACK 및 재전송**하는 구조를 설명합니다.

### 🔹 동작 단계별 해석

#### ① 초기 전송

`송신자(sender): pkt0, pkt1, pkt2, pkt3 전송 수신자(receiver):   - pkt0 수신 → ack0 전송 - pkt1 수신 → ack1 전송 - pkt2 손실(loss 발생)`

→ pkt2 손실로 인해 이후 pkt3, pkt4, pkt5는 순서가 어긋난(out-of-order) 상태로 도착합니다.

#### ② 순서가 어긋난 패킷 수신

`receiver:  - pkt3 수신 → 버퍼(buffer)에 저장, ack3 전송 - pkt4 수신 → 버퍼 저장, ack4 전송 - pkt5 수신 → 버퍼 저장, ack5 전송`

→ SR에서는 **순서가 어긋난 패킷도 버퍼에 저장**하며,  
각 패킷마다 개별 ACK을 보냅니다.  
(Go-Back-N에서는 이 단계에서 패킷들을 모두 버렸을 것입니다.)

#### ③ 송신자 측 ACK 처리

`sender:  - ack0, ack1, ack3 수신 - pkt2에 대한 ACK 없음 → pkt2의 타이머 만료(timeout) - pkt2만 재전송 (but not 3,4,5)`

→ SR의 핵심: **손실된 패킷만 선택적으로 재전송**합니다.  
다른 패킷(3,4,5)은 이미 ACK를 받았기 때문에 재전송하지 않습니다.

#### ④ 재전송된 패킷 수신

`receiver:  - pkt2 수신 → deliver(pkt2, pkt3, pkt4, pkt5) - ack2 전송`

→ pkt2가 도착하면서 **버퍼에 저장된 3~5번 패킷이 순서대로 상위 계층에 전달**됩니다.  
(이때 순서가 완전히 복구되어 정상적으로 데이터가 이어짐)

### 🔹 핵심 개념 요약

| 항목                  | 설명                                                |
| ------------------- | ------------------------------------------------- |
| **윈도우 크기**          | N = 4                                             |
| **ACK 방식**          | 각 패킷마다 개별 ACK 전송                                  |
| **손실 처리**           | 손실된 패킷만 재전송 (pkt2)                                |
| **수신자 처리**          | out-of-order 패킷을 버퍼에 저장 후, 순서 완성 시 일괄 전달          |
| **Go-Back-N과의 차이점** | 전체 재전송이 아닌 “선택적 재전송(Selective Retransmission)” 수행 |

### 🧩 결론

Selective Repeat은 **네트워크 효율을 극대화**하는 신뢰적 전송 방식으로,  
패킷 손실이 발생하더라도 **해당 패킷만 개별적으로 재전송**하여  
대역폭 낭비를 줄이고, 전송 지연을 최소화합니다.

> 💡 슬라이드의 질문 “What happens when ack2 arrives?”  
> → ACK2가 도착하면, 송신자의 윈도우가 앞으로 이동하며  
> pkt6, pkt7 등의 새로운 패킷 전송이 가능해집니다.



