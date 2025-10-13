

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


