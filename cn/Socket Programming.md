몇개는 넘어간다고했던거 같은데
### 📘 **Socket Programming (소켓 프로그래밍)**

#### **목표 (Goal)**

소켓을 이용하여 **클라이언트/서버 애플리케이션 간 통신이 가능한 프로그램을 구현하는 방법**을 학습하는 것.

#### **소켓의 정의 (Socket)**

> **소켓(socket)**:  
> 애플리케이션 프로세스(application process)와 **종단 간 전송 프로토콜(end-to-end transport protocol)** 사이를 연결하는 **문(door)** 역할을 함.

즉, 소켓은 응용 계층과 전송 계층을 이어주는 **인터페이스**로, 데이터를 송수신할 수 있도록 함.

#### **그림 설명**

- 각 컴퓨터(클라이언트와 서버)는 **프로세스(process)** 가 실행 중이며,  
    해당 프로세스는 **소켓(socket)** 을 통해 네트워크로 연결됨.
 
- 소켓을 통해 애플리케이션이 전송 계층(예: TCP, UDP)에 접근하여 데이터를 주고받음.    
- **제어 영역 구분:**
    - **Application layer (상위 부분):**  
        → **개발자(app developer)** 가 제어
    - **Transport 이하 계층:**  
        → **운영체제(OS)** 가 제어

### 🧭 **요약**

- **소켓(Socket):** 응용 프로그램이 네트워크 통신을 수행하기 위한 **논리적 통신 창구**    
- **역할:** 애플리케이션과 전송 계층 간의 **데이터 송수신 인터페이스**
- **책임 분담:**
    - 개발자는 애플리케이션 로직과 소켓 동작을 설계
    - 운영체제는 전송·네트워크 계층에서 데이터 전달을 담당

### 📘 **Socket (소켓)**

#### **1. 각 소켓의 구성 요소 (Each socket is associated with five components)**

하나의 소켓은 다음 **5가지 요소**와 연관되어 있습니다.

1. **Protocol (프로토콜)**    
    - 프로토콜 패밀리(protocol family) 및 실제 프로토콜 종류
    - 예: IPv4 + TCP, IPv4 + UDP 등

2. **Source address (출발지 주소)**    
3. **Source port (출발지 포트)**
4. **Destination address (목적지 주소)**
5. **Destination port (목적지 포트)**

#### **2. 구성 요소 정의 위치 (Where to define components)**

| 구성 요소                         | 정의 함수                     | 설명                          |
| ----------------------------- | ------------------------- | --------------------------- |
| **Protocol**                  | `socket()`                | 사용할 전송 프로토콜(TCP, UDP 등)을 지정 |
| **Source address, port**      | `bind()`                  | 소켓이 사용할 로컬 주소 및 포트를 명시      |
| **Destination address, port** | `connect()` 또는 `sendto()` | 통신 대상(서버)의 주소 및 포트를 설정      |

### 🧭 **요약**

- 소켓은 네트워크 통신의 **논리적 통로(logical endpoint)** 로서, 프로토콜·주소·포트 정보를 통해 연결을 정의함.    
- 개발자는 `socket()`, `bind()`, `connect()` 등의 함수를 이용해 소켓의 구성 요소를 초기화하고 네트워크 통신을 수행함.


### 📘 **TCP: Overview (전송 제어 프로토콜 개요)**

**RFCs:** 793, 1122, 2018, 5681, 7323
#### **1. 기본 특성 (Core Characteristics)**

- **point-to-point:**  
    → **하나의 송신자(sender)** 와 **하나의 수신자(receiver)** 간 통신만 지원함.
- **reliable, in-order byte stream:**  
    → 신뢰성 있고 순서가 보장된 바이트 스트림 전송.  
    → “메시지 경계(message boundaries)”가 존재하지 않음 (데이터를 연속된 바이트 흐름으로 처리).    
- **full duplex data:**  
    → 동일 연결 내에서 양방향 데이터 전송 가능.  
    → **MSS (Maximum Segment Size):** 한 세그먼트에 포함될 수 있는 데이터의 최대 크기.

#### **2. 주요 동작 방식 (Operational Mechanisms)**

- **cumulative ACKs (누적 확인 응답):**  
    → 여러 패킷에 대한 수신 확인을 하나의 ACK로 처리하여 효율성을 높임.
- **pipelining:**  
    → 여러 세그먼트를 동시에 전송 가능.  
    → TCP의 **혼잡 제어(congestion control)** 및 **흐름 제어(flow control)** 에 따라 윈도우 크기(window size)가 결정됨.
- **connection-oriented (연결 지향형):**  
    → 송신자와 수신자는 데이터 전송 전에 **3-way handshake** 등 제어 메시지 교환을 통해 상태를 초기화함.    
- **flow controlled (흐름 제어):**  
    → 송신자가 너무 빠르게 전송하지 않도록 하여 **수신 측의 버퍼 초과를 방지함**.

### 🧭 **요약**

- TCP는 **신뢰성(reliability)**, **순서 보장(order)**, **양방향 통신(full-duplex)**, **흐름 및 혼잡 제어(flow & congestion control)** 를 특징으로 하는 **연결 지향형 전송 프로토콜**임.
- 데이터는 **스트림 형태**로 전달되며, 전송 중 손실이 발생하더라도 **재전송 및 ACK** 메커니즘으로 복구됨.


별표
### 📘 **TCP Server/Client Function Call (TCP 서버/클라이언트 함수 호출 과정)**

#### **서버 측(Server)**

1. **`socket()`** — 소켓 생성  
    → 통신을 위한 소켓(네트워크 인터페이스)을 생성함.

2. **`bind()`** — 소켓에 주소 할당  
    → IP 주소와 포트 번호를 소켓에 연결(bind)함.

3. **`listen()`** — 연결 요청 대기  
    → 클라이언트의 접속 요청을 기다리는 **수신 대기 상태**로 전환.
   
4. **`accept()`** — 연결 요청 수락  
    → 클라이언트의 연결 요청을 수락하고, 새 통신용 소켓 생성.
 
5. **`read()/write()`** — 데이터 송수신  
    → 클라이언트와 데이터를 교환(수신/전송).
   
6. **`close()`** — 연결 종료  
    → 통신이 끝난 후 연결을 닫고 자원을 해제.
 

#### **클라이언트 측(Client)**

1. **`socket()`** — 소켓 생성  
    → 통신을 위한 클라이언트 소켓을 생성함.
   
2. **`connect()`** — 서버로 연결 요청  
    → 지정된 서버(IP, 포트)에 연결을 요청함.
 
3. **`read()/write()`** — 데이터 송수신  
    → 서버와 데이터를 교환(전송/수신).
   
4. **`close()`** — 연결 종료  
    → 통신 완료 후 소켓을 닫음.
 

#### **데이터 흐름 요약**

- **Connection Request (연결 요청):**  
    클라이언트 → 서버 (`connect()` ↔ `accept()`)

- **Send/Receive (데이터 송수신):**  
    서버 ↔ 클라이언트 (`read()/write()`)
   
- **Connection Close (연결 종료):**  
    양쪽에서 `close()` 호출
 

### 🧭 **요약**

TCP 통신은 **연결 지향형(connection-oriented)** 프로토콜로,  
서버와 클라이언트가 다음 순서로 상호 작용합니다:

> **서버:** `socket()` → `bind()` → `listen()` → `accept()` → `read()/write()` → `close()`  
> **클라이언트:** `socket()` → `connect()` → `read()/write()` → `close()`

이 과정을 통해 TCP는 **신뢰성 있고 순서가 보장된** 데이터 전송을 수행합니다.


## 🧩 `socket()` 함수

`#include <sys/socket.h> int socket(int domain, int type, int protocol);`

### 1. 개요

`socket()` 함수는 **통신의 끝단(endpoint)을 생성**하여 프로세스 간 네트워크 통신을 가능하게 합니다.  
성공 시 **새 소켓의 파일 디스크립터(fd)** 를 반환하고, 실패 시 **-1**을 반환합니다.

### 2. 매개변수 설명

|인자|의미|예시|
|---|---|---|
|**domain**|사용할 **프로토콜 패밀리(Protocol Family)**|`PF_INET` : IPv4  <br>`PF_INET6` : IPv6|
|**type**|**서비스의 종류(Type of Service)**|`SOCK_STREAM` : TCP (연결지향형)  <br>`SOCK_DGRAM` : UDP (비연결형)  <br>`SOCK_RAW` : Raw IP|
|**protocol**|사용할 **특정 프로토콜 번호** (일반적으로 0)|`IPPROTO_TCP` 또는 `IPPROTO_UDP`|

> 💡 보통 `protocol` 인자는 `0`으로 설정합니다.  
> 이는 `type` 인자에 따라 기본 프로토콜(TCP 또는 UDP)이 자동으로 선택됨을 의미합니다.

### 3. 반환값 (Return Value)

- **성공 시**: 새 소켓의 **파일 디스크립터** (정수 값)
- **실패 시**: `-1` 반환  
    → 에러 발생 시 `perror()` 또는 `errno`를 통해 원인 확인 가능


### 4. 사용 예시

#### ✅ TCP 소켓 생성

`int sock = socket(PF_INET, SOCK_STREAM, 0); if (sock == -1)     error_handling("socket() error");`

#### ✅ UDP 소켓 생성

`int sock = socket(PF_INET, SOCK_DGRAM, 0); if (sock == -1)     error_handling("socket() error");`


## 🌐 Internet Address (인터넷 주소)

### 1. IP Address (IP 주소)

**정의:**  
인터넷에서 **컴퓨터를 식별하기 위한 주소 체계**입니다.

- 컴퓨터를 네트워크 상에서 식별하기 위해 사용됩니다.
- **IPv4**: 4바이트(32비트) 주소 체계  
    → 예: `192.168.0.1`
- **IPv6**: 16바이트(128비트) 주소 체계  
    → 예: `2001:0db8::1`    
- 소켓을 생성할 때, **기본 프로토콜(IP 버전)** 을 지정해야 합니다.  
    (`PF_INET` 또는 `PF_INET6` 등)

### 2. Port (포트)

**정의:**  
한 컴퓨터 내에서 여러 통신 채널(소켓)을 구분하기 위한 **식별 번호**입니다.

- 컴퓨터 내부에서 소켓을 구분하는 데 사용됩니다.
- 포트 번호는 **16비트 정수**로 표현됩니다.  
    → 전체 범위: **0 ~ 65535**

### 3. 포트의 종류

#### ▪ Well-known Port (잘 알려진 포트)

- 범위: **0 ~ 1023**    
- 이미 특정 서비스용으로 예약되어 있음.  
    예시:
    - `22`: SSH (Secure Shell)
    - `80`: HTTP (웹 서버)
    - `443`: HTTPS

#### ▪ Ephemeral Port (임시 포트)

- 단기간 사용되는 전송용 포트
- 운영체제가 자동으로 할당함
- 예시 (리눅스 기준): **32768 ~ 60999**
- 클라이언트 프로그램이 서버와 통신할 때 일시적으로 생성되는 포트

### 🧭 요약

| 구분              | 범위                        | 사용 예시             | 특징         |
| --------------- | ------------------------- | ----------------- | ---------- |
| Well-known Port | 0 ~ 1023                  | 22(SSH), 80(HTTP) | 고정, 서비스 지정 |
| Registered Port | 1024 ~ 49151              | 사용자 정의 서비스        | 등록 가능      |
| Ephemeral Port  | 49152 ~ 65535 (또는 OS별 다름) | 클라이언트 임시 통신       | 자동 할당      |

