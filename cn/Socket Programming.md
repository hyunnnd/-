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
<<<<<<< HEAD
| ----------------------------- | ------------------------- | --------------------------- |
=======
| -- | - |  |
>>>>>>> origin/main
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
<<<<<<< HEAD
|---|---|---|
=======
||||
>>>>>>> origin/main
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
<<<<<<< HEAD
| --------------- | ------------------------- | ----------------- | ---------- |
=======
|  | - | -- | - |
>>>>>>> origin/main
| Well-known Port | 0 ~ 1023                  | 22(SSH), 80(HTTP) | 고정, 서비스 지정 |
| Registered Port | 1024 ~ 49151              | 사용자 정의 서비스        | 등록 가능      |
| Ephemeral Port  | 49152 ~ 65535 (또는 OS별 다름) | 클라이언트 임시 통신       | 자동 할당      |

## 🧱 Address Structure (IPv4)

### 1. 개요

IPv4 통신에서 소켓 주소 정보를 저장하기 위한 구조체는 `<netinet/in.h>`에 정의되어 있습니다. 
이 구조체는 소켓을 특정 IP 주소와 포트 번호에 **바인딩(bind)** 할 때 사용됩니다.


### 2. 구조체 정의

`struct sockaddr_in {     sa_family_t    sin_family;   // 주소 체계 (Address family): AF_INET     in_port_t      sin_port;     // 포트 번호 (16비트), 네트워크 바이트 순서     struct in_addr sin_addr;     // IP 주소 (32비트), 네트워크 바이트 순서     char           sin_zero[8];  // 사용되지 않음 (패딩) };`

#### 하위 구조체:

`struct in_addr {     in_addr_t s_addr;            // IPv4 주소 (32비트) };`

> **참고:**  
> `sin_zero`는 구조체 크기를 맞추기 위한 **패딩용 필드**로 실제 사용되지 않습니다.

### 3. 구조체 비교

|구조체|정의 헤더|주요 역할|
<<<<<<< HEAD
|---|---|---|
=======
||||
>>>>>>> origin/main
|`sockaddr_in`|`<netinet/in.h>`|IPv4용 주소 정보 저장|
|`sockaddr`|`<sys/socket.h>`|프로토콜 독립적 주소 구조 (범용 포인터로 사용됨)|

#### 범용 구조체 예시

`struct sockaddr {     sa_family_t sa_family;   // 주소 체계 (예: AF_INET)     char        sa_data[14]; // 주소 데이터 (포트 + IP 등) };`

> 대부분의 소켓 함수(`bind()`, `connect()` 등)는 `sockaddr_in` 대신  
> `sockaddr *` 형태의 포인터를 인자로 받습니다.  
> 따라서 `struct sockaddr_in` → `(struct sockaddr *)`로 형 변환이 필요합니다.

### 4. 예시 코드 설명

`struct sockaddr_in serv_addr; memset(&serv_addr, 0, sizeof(serv_addr));  serv_addr.sin_family = AF_INET;             // IPv4 주소 체계 지정 serv_addr.sin_addr.s_addr = htonl(INADDR_ANY); // 모든 IP에서 수신 허용 serv_addr.sin_port = htons(atoi(argv[1]));     // 명령행 인자로 포트 입력받음  if (bind(serv_sock, (struct sockaddr*)&serv_addr, sizeof(serv_addr)) == -1)     error_handling("bind() error");`

#### 주요 포인트:

- `htonl()`, `htons()`:  
    호스트 바이트 순서(Host Byte Order)를 네트워크 바이트 순서(Network Byte Order)로 변환합니다.
    
    - `htonl()`: 32비트용 (IP 주소)
    - `htons()`: 16비트용 (포트 번호)
- `INADDR_ANY`:  
    특정 IP가 아닌, **모든 로컬 인터페이스**에서 오는 연결을 수락합니다.


## 🔄 Byte Order Conversion (바이트 순서 변환)

### 1. Byte Ordering (바이트 정렬 방식)

컴퓨터는 다바이트 데이터를 메모리에 저장할 때 **바이트 순서**를 다르게 사용할 수 있습니다.

| 구분                | 정의                                          | 저장 순서 (예: 0A 0B 0C 0D) | 설명                                |
<<<<<<< HEAD
| ----------------- | ------------------------------------------- | ---------------------- | --------------------------------- |
=======
| -- | - | - |  |
>>>>>>> origin/main
| **Little Endian** | Least Significant Byte first (하위 바이트 먼저 저장) | `0D 0C 0B 0A`          | Intel CPU 계열 등에서 사용               |
| **Big Endian**    | Most Significant Byte first (상위 바이트 먼저 저장)  | `0A 0B 0C 0D`          | 네트워크 전송 시 사용 (Network Byte Order) |

### 2. Network Byte Order (네트워크 바이트 순서)

- **정의:**  
    네트워크에서 데이터를 전송할 때는 모든 시스템 간 호환을 위해  
    **Big Endian 방식**을 표준으로 사용합니다.  
    → 즉, **Network Byte Order = Big Endian**
- **특징:**
    
    - 가장 큰 자리 바이트(MSB, Most Significant Byte)가 메모리의 낮은 주소(x)에 위치
    - CPU의 구조에 따라 Host Byte Order는 다를 수 있음  
        (예: Intel = Little Endian, 일부 네트워크 장비 = Big Endian)

### 3. 시각적 예시

#### 32비트 정수 (예: 0x0A0B0C0D)

| 메모리 주소            | x   | x+1 | x+2 | x+3 |
<<<<<<< HEAD
| ----------------- | --- | --- | --- | --- |
=======
| -- |  |  |  |  |
>>>>>>> origin/main
| **Big Endian**    | 0A  | 0B  | 0C  | 0D  |
| **Little Endian** | 0D  | 0C  | 0B  | 0A  |

> **Big Endian:** 포인터가 “큰 쪽” (MSB)부터 가리킴  
> **Little Endian:** 포인터가 “작은 쪽” (LSB)부터 가리킴

### 4. Host vs Network Conversion 함수

네트워크 프로그램에서는 **엔디안 차이를 자동으로 변환**하기 위해  
다음 함수를 사용합니다. (`<arpa/inet.h>` 헤더 포함)

| 함수명       | 역할                            | 변환 방향          |
<<<<<<< HEAD
| --------- | ----------------------------- | -------------- |
=======
|  | -- | -- |
>>>>>>> origin/main
| `htons()` | Host to Network Short (16bit) | Host → Network |
| `htonl()` | Host to Network Long (32bit)  | Host → Network |
| `ntohs()` | Network to Host Short         | Network → Host |
| `ntohl()` | Network to Host Long          | Network → Host |

### 🧭 요약

- **Host Byte Order:** CPU 아키텍처에 따라 다름 (예: x86 = Little Endian)    
- **Network Byte Order:** 항상 **Big Endian**
- **데이터 송수신 시 변환 필수:** `htons()`, `htonl()` 등 사용


## 🌐 `inet_pton()` 함수

### 1. 개요

**헤더 파일**

`#include <arpa/inet.h>`

**함수 원형**

`int inet_pton(int af, const char *restrict src, void *restrict dst);`

**설명:**  
`inet_pton()` 함수는 **문자열 형태의 IPv4 또는 IPv6 주소를**  
**이진(binary) 네트워크 바이트 순서로 변환**합니다.

즉, `"192.168.0.1"` → `0xC0A80001` 형태로 바꿔줍니다.  
이진 주소는 소켓 구조체(`struct in_addr`, `struct in6_addr`)에 저장됩니다.

### 2. 매개변수 설명

| 매개변수    | 자료형            | 설명                                                           |
<<<<<<< HEAD
| ------- | -------------- | ------------------------------------------------------------ |
=======
| - | -- |  |
>>>>>>> origin/main
| **af**  | `int`          | 주소 체계(Address Family): `AF_INET` (IPv4) 또는 `AF_INET6` (IPv6) |
| **src** | `const char *` | 문자열 형태의 IP 주소                                                |
| **dst** | `void *`       | 변환된 네트워크 주소를 저장할 구조체 포인터                                     |

### 3. 반환값

| 값      | 의미                                      |
<<<<<<< HEAD
| ------ | --------------------------------------- |
=======
|  |  |
>>>>>>> origin/main
| **1**  | 변환 성공                                   |
| **0**  | 주소 문자열이 잘못됨 (Invalid address string)    |
| **-1** | 주소 체계가 유효하지 않음 (Invalid address family) |


### 4. 예제 코드

`const char *ip1 = "1.2.3.4"; const char *ip2 = "239.1.2.3"; struct in_addr addr1, addr2;  if (inet_pton(AF_INET, ip1, &addr1) <= 0)     perror("inet_pton IPv4 error"); else     printf("IP#1's binary: 0x%08x\n", addr1.s_addr);  if (inet_pton(AF_INET, ip2, &addr2) <= 0)     perror("inet_pton IPv4 error"); else     printf("IP#2's binary: 0x%08x\n", addr2.s_addr);`

**출력 결과**

`IP#1's binary: 0x04030201 IP#2's binary: 0x030201ef`

### 5. 동작 원리

- `inet_pton()`은 “Presentation to Numeric”의 약어입니다.  
    (문자열 표현 → 숫자 표현)    
- `inet_ntop()`은 반대 기능을 수행합니다.  
    (Numeric → Presentation)
- 내부적으로 `htons()`/`htonl()` 변환이 적용되어  
    **네트워크 바이트 순서(Big Endian)** 로 저장됩니다.


## 🧭 `getaddrinfo()` 함수

### 1. 개요

**헤더 파일**

`#include <sys/types.h> #include <sys/socket.h> #include <netdb.h>`

**함수 원형**

`int getaddrinfo(     const char *restrict node,      const char *restrict service,     const struct addrinfo *restrict hints,     struct addrinfo **restrict res );`

**설명:**  
`getaddrinfo()` 함수는 **호스트 이름(또는 IP 주소 문자열)** 과  
**서비스 이름(또는 포트 번호)** 을 **소켓 주소 구조체(`struct addrinfo`) 리스트**로 변환합니다.  
즉, 사람이 읽을 수 있는 주소를 소켓에서 사용할 수 있는 형태로 바꿔주는 함수입니다.

> `getnameinfo()`는 반대 기능(주소 → 이름)을 수행합니다.


### 2. 매개변수 설명

|매개변수|자료형|설명|
<<<<<<< HEAD
|---|---|---|
=======
||||
>>>>>>> origin/main
|**node**|`const char *`|호스트 이름 또는 IP 주소 문자열 (예: `"www.google.com"` 또는 `"192.168.0.1"`)|
|**service**|`const char *`|서비스 이름 또는 포트 번호 (예: `"http"` 또는 `"80"`)|
|**hints**|`const struct addrinfo *`|(선택사항) 주소 패밀리, 소켓 타입 등 설정을 담은 구조체|
|**res**|`struct addrinfo **`|변환 결과를 저장할 연결 리스트의 시작 주소 (성공 시 메모리 할당됨)|

> `res`는 사용 후 반드시 `freeaddrinfo(res)`로 해제해야 합니다.

### 3. 반환값

|반환값|의미|
<<<<<<< HEAD
|---|---|
=======
|||
>>>>>>> origin/main
|**0**|성공|
|**비 0 값**|오류 (에러 코드 반환, `gai_strerror()`로 문자열 변환 가능)|


### 4. 구조체 정의

`struct addrinfo {     int              ai_flags;      // 옵션 플래그     int              ai_family;     // 주소 체계 (AF_INET, AF_INET6, AF_UNSPEC)     int              ai_socktype;   // 소켓 타입 (SOCK_STREAM, SOCK_DGRAM)     int              ai_protocol;   // 프로토콜 (IPPROTO_TCP, IPPROTO_UDP)     socklen_t        ai_addrlen;    // 주소 길이     struct sockaddr *ai_addr;       // 실제 주소 정보     char            *ai_canonname;  // 정식 호스트 이름 (canonical name)     struct addrinfo *ai_next;       // 다음 노드 (연결 리스트 형태) };`

### 5. 사용 예시

`struct addrinfo hints, *res; memset(&hints, 0, sizeof(hints)); hints.ai_family = AF_INET;        // IPv4 hints.ai_socktype = SOCK_STREAM;  // TCP  if (getaddrinfo("example.com", "80", &hints, &res) != 0) {     perror("getaddrinfo() error");     exit(1); }  // res를 사용하여 socket(), connect() 등 수행 가능  freeaddrinfo(res);  // 메모리 해제`

### 6. 특징 및 장점

- **IPv4/IPv6를 모두 지원** (주소 체계 자동 구분 가능)
- **DNS 이름 해석 기능 포함** (호스트 이름 → IP 변환)
- 여러 주소 후보를 **연결 리스트로 반환**하여 유연한 소켓 설정 가능
- 고전 함수인 `gethostbyname()`과 `getservbyname()`을 대체

<<<<<<< HEAD
=======

## TCP에는 메시지 경계가 없음 중요

- **“메시지 경계(message boundary)”**란, 네트워크 프로토콜을 통해 전송되는 두 개의 메시지를 구분하는 경계를 의미함.
- **UDP는 메시지 경계를 보존함.**
    - 예를 들어, 서버가 `write()`를 두 번 호출하면, 클라이언트는 각각의 메시지를 받기 위해 `read()`를 두 번 호출해야 함.

- **TCP는 메시지 경계를 보존하지 않음.**    
    - 예를 들어, 서버가 `write()`를 두 번 호출하더라도, 클라이언트는 한 번의 `read()` 호출로 모든 데이터를 한꺼번에 읽을 수 있음.

### 서버 코드 예시

`char message1[] = "Hello World!111"; char message2[] = "Hello World!222"; write(clnt_sock, message1, sizeof(message1)); write(clnt_sock, message2, sizeof(message2));`

### 클라이언트 코드 예시

`sleep(1); read_len = read(sock, message, sizeof(message)); printf("Message from server: "); for (i = 0; i < sizeof(message); i++)     printf("%c", message[i]); printf("Read_len: %d\n", read_len);`

### 실행 결과

`Message from server: Hello World!111Hello World!222 Read_len: 32`

**설명:**  
서버는 두 번 `write()`를 호출했지만, 클라이언트는 한 번의 `read()`로 두 메시지를 연속된 데이터로 받음.  
이는 TCP가 데이터를 “메시지 단위”가 아닌 “연속된 바이트 스트림”으로 처리하기 때문임.  
따라서 TCP에서는 메시지의 시작과 끝을 구분하려면 응용 계층에서 직접 구분자를 추가해야 함.


## 서버의 유형 (Server Type)

### 🔹 반복형 서버 (Iterative Server)

- 하나의 서버 프로세스가 “well-known port(잘 알려진 포트)”에서 들어오는 요청을 순차적으로 받아 처리함.
- 즉, 한 번에 **하나의 클라이언트만 처리**할 수 있으며, 이전 요청 처리가 끝나야 다음 요청을 받을 수 있음.
- 대부분의 **UDP 서버는 반복형(Iterative)** 구조를 가짐.



### 🔹 동시형 서버 (Concurrent Server)

- 각 클라이언트마다 **별도의 프로세스(process)** 또는 **스레드(thread)** 를 생성하여 처리함.
- 메인 서버 프로세스는 클라이언트의 연결 요청을 받을 때마다 새로운 서비스 프로세스를 만들어 이를 담당시킴.
- 대부분의 **TCP 서버는 동시형(Concurrent)** 구조로, 여러 클라이언트의 연결을 동시에 관리하기에 유리함.

### 🔸 서버 동작 절차 (오른쪽 그림)

1. `socket()` – 소켓 생성    
2. `bind()` – 서버 주소(포트 포함) 바인딩
3. `listen()` – 클라이언트의 연결 요청 대기
4. `accept()` – 연결 수락
5. `read()` / `write()` – 데이터 송수신
6. `close(client)` – 클라이언트 연결 종료
7. `close(server)` – 서버 종료

**요약:**

- **Iterative Server:** 단일 프로세스, 요청을 순차적으로 처리 → 단순하지만 동시에 여러 요청을 처리할 수 없음.    
- **Concurrent Server:** 요청마다 별도의 프로세스 생성 → 복잡하지만 다중 클라이언트 처리에 효율적임.


## 반복형 서버 vs 동시형 서버 (Iterative vs. Concurrent Servers)

### 🔹 반복형 서버 구조 (Iterative Server Skeleton)

`int sockfd, newsockfd;  if ((sockfd = socket(...)) < 0)     err_sys("socket error"); if ((bind(sockfd, ...)) < 0)     err_sys("bind error"); if (listen(sockfd, 5))     err_sys("listen error");  for (;;) {     newsockfd = accept(sockfd, ...);     if (newsockfd < 0)         err_sys("accept error");      doit(newsockfd);      // 클라이언트 요청 처리     close(newsockfd);     // 처리 완료 후 연결 종료 }`

**설명:**

- 하나의 서버 프로세스가 모든 요청을 **순차적으로 처리함.**
- 새로운 연결 요청은 이전 요청이 끝날 때까지 대기해야 함.
- 구조가 단순하지만 **동시 접속이 불가능**함.
- 주로 **UDP 서버**에서 사용됨.

### 🔹 동시형 서버 구조 (Concurrent Server Skeleton)

`int sockfd, newsockfd;  if ((sockfd = socket(...)) < 0)     err_sys("socket error"); if ((bind(sockfd, ...)) < 0)     err_sys("bind error"); if (listen(sockfd, 5))     err_sys("listen error");  for (;;) {     newsockfd = accept(sockfd, ...);     if (newsockfd < 0)         err_sys("accept error");      if (fork() == 0) {      // 자식 프로세스 생성         close(sockfd);      // 부모 소켓 닫기         doit(newsockfd);    // 클라이언트 요청 처리         exit(0);            // 자식 프로세스 종료     } else {         close(newsockfd);   // 부모는 연결 소켓 닫음     } }`

**설명:**

- 클라이언트 연결마다 **새로운 프로세스(fork)** 를 생성하여 병렬로 처리함.    
- 메인 프로세스는 계속해서 `accept()`를 호출하며 새로운 클라이언트를 받을 수 있음.
- 각 클라이언트는 독립적으로 처리되어, **여러 요청을 동시에 처리 가능함.**
- 주로 **TCP 서버**에서 사용됨.

### ⚙️ 요약 비교

| 구분        | Iterative Server   | Concurrent Server  |
|  |  |  |
| 구조        | 단일 프로세스            | 클라이언트마다 자식 프로세스 생성 |
| 동시 처리     | 불가능                | 가능                 |
| 복잡도       | 단순                 | 비교적 복잡             |
| 주 사용 프로토콜 | UDP                | TCP                |
| 장점        | 구현이 간단, 자원 소모 적음   | 다중 접속 처리에 유리       |
| 단점        | 클라이언트가 많을 경우 지연 발생 | 프로세스/스레드 생성 오버헤드   |

**핵심 요약:**

- **Iterative 서버**는 “하나씩 순서대로 처리하는 구조”    
- **Concurrent 서버**는 “클라이언트마다 병렬로 처리하는 구조”
- TCP 서버는 대부분 **Concurrent**, UDP 서버는 대부분 **Iterative** 방식임.


## TCP 소켓의 입력 버퍼(Input Buffer)와 출력 버퍼(Output Buffer)

### 🔹 개요

- TCP 소켓에는 **입력 버퍼(input buffer)** 와 **출력 버퍼(output buffer)** 가 존재함.    
- 두 버퍼는 **소켓이 생성될 때 자동으로 함께 생성**됨.

### 🔸 동작 원리

- **입력 버퍼(Input Buffer)**:  
    수신 측에서 데이터를 임시로 저장하는 공간임.  
    클라이언트(또는 서버)가 `read()`를 호출하면 입력 버퍼에 저장된 데이터를 읽어옴.    
- **출력 버퍼(Output Buffer)**:  
    송신 측에서 보낼 데이터를 임시로 저장하는 공간임.  
    `write()`를 호출하면 데이터가 출력 버퍼에 저장되고, 커널이 이를 네트워크를 통해 전송함.

### 🔸 소켓 종료 시 동작

- 소켓이 **닫혀도 출력 버퍼에 남은 데이터**는 계속 전송됨.  
    → 이미 커널에 전달된 데이터는 TCP의 신뢰성 보장에 따라 끝까지 송신됨.    
- 반면, 소켓이 닫히면 **입력 버퍼에 남은 데이터는 즉시 삭제됨.**  
    → 더 이상 읽히지 못하고 소멸됨.

### 🔸 그림 설명

- 왼쪽 컴퓨터의 **출력 버퍼(Output buffer)** 는 오른쪽 컴퓨터의 **입력 버퍼(Input buffer)** 로 데이터를 전송함.    
- 반대로 오른쪽의 출력 버퍼도 왼쪽의 입력 버퍼로 데이터를 전달함.
- `write()`는 데이터를 **출력 버퍼로 넣는 동작**,  
    `read()`는 데이터를 **입력 버퍼에서 꺼내는 동작**을 의미함.

**정리:**  
TCP 통신은 애플리케이션이 직접 데이터를 주고받는 것이 아니라,  
운영체제가 관리하는 **버퍼를 통해 간접적으로 송수신**함.  
이로 인해 송수신 속도의 차이가 있어도 데이터 손실 없이 안정적인 통신이 가능함.


## Half-close (하프 클로즈)

### 🔹 `close()`의 의미

- `close()`는 **소켓을 완전히 종료(destruction)** 시키는 동작을 의미함.
- 한 번 `close()`가 호출되면 **더 이상 입출력(I/O) 작업이 불가능**함.
- 아직 데이터 송수신이 완료되지 않은 상태에서 `close()`를 호출하면 **전송 중단, 데이터 손실 등 문제**가 발생할 수 있음.

### 🔹 Half-close (부분 종료)

- **입력 스트림(input stream)** 또는 **출력 스트림(output stream)** 중 **하나만 닫는 동작**을 의미함. 
- 즉, 송신은 중단하되 수신은 계속할 수 있음(또는 그 반대).
- TCP에서는 `shutdown()` 함수를 사용하여 이를 구현함.
    예:
    `shutdown(sock, SHUT_WR);  // 송신(write)만 종료`
    → 이후에는 데이터를 보낼 수 없지만,  
    반대편에서 오는 데이터(`read`)는 여전히 받을 수 있음.

### 🔸 그림 설명

- 왼쪽 컴퓨터가 `shutdown(sock, SHUT_WR)`을 호출하면,  
    왼쪽의 **출력 버퍼(Output buffer)** 는 닫히지만 **입력 버퍼(Input buffer)** 는 그대로 유지됨.   
- 따라서 오른쪽 컴퓨터는 여전히 데이터를 전송할 수 있고,  
    왼쪽은 그 데이터를 계속 **read()** 로 받을 수 있음.

### ⚙️ 요약

| 구분           | close()       | half-close()      |
|  | - | -- |
| 종료 범위        | 입력 + 출력 모두 종료 | 입력 또는 출력 중 하나만 종료 |
| 사용 함수        | `close()`     | `shutdown()`      |
| 추가 송수신 가능 여부 | 불가능           | 한쪽 방향만 가능         |
| 특징           | 완전한 연결 종료     | 한쪽 방향만 통신 유지 가능   |

**정리:**  
Half-close는 TCP의 **양방향 통신을 한쪽 방향만 종료**하는 기능으로,  
데이터 전송은 끝났지만 수신은 계속해야 하는 상황(예: 파일 전송 완료 후 확인 메시지 수신 등)에 유용하게 사용됨.


## `shutdown()` 함수

### 🔹 함수 원형

`#include <unistd.h> int shutdown(int socket, int how);`


### 🔸 개요

- `shutdown()` 함수는 **소켓의 송신(send) 또는 수신(receive) 기능을 부분적으로 종료**시키는 함수임.
- 즉, TCP의 **full-duplex 통신(양방향 통신)** 중 한쪽 방향만 차단할 수 있음.
- 완전히 소켓을 제거하는 것이 아니라, **입출력 기능만 제한**함.  
    → 실제 파일 디스크립터를 해제하려면 `close()`를 추가로 호출해야 함.

### 🔸 매개변수 설명

- **socket:**  
    종료 대상이 되는 소켓의 파일 디스크립터
- **how:**  
    종료할 방향을 지정함
    |상수|값|의미|
    ||||
    |`SHUT_RD`|0|더 이상 **수신(receive)** 불가능|
    |`SHUT_WR`|1|더 이상 **송신(send)** 불가능|
    |`SHUT_RDWR`|2|송신과 수신 모두 불가능|
    

### 🔸 반환값

- **성공:** `0`    
- **오류:** `-1`

### 🔹 예제 코드

`shutdown(clnt_sd, SHUT_WR);           // 송신 기능만 종료 read(clnt_sd, buf, BUF_SIZE);         // 여전히 수신은 가능 printf("Message from client: %s\n", buf);  close(clnt_sd);  close(serv_sd);`

### ⚙️ 정리

- `shutdown()`은 TCP 연결의 **한쪽 방향만 닫을 수 있는 제어 함수**임.    
- 완전한 종료(`close()`) 전에 송신이나 수신을 단계적으로 중단할 수 있음.
- 예를 들어, **데이터 전송을 모두 끝냈지만 상대방의 응답을 계속 받아야 하는 경우**,  
    `shutdown(sock, SHUT_WR)`을 사용함.


## UDP 서버/클라이언트 함수 호출 과정 (UDP Server/Client Function Call)



### 🔹 서버(Server)

1. **socket()**
    - 소켓 생성.
    - `AF_INET`, `SOCK_DGRAM` 형식으로 UDP 소켓을 만듦.

2. **bind()**    
    - 서버의 IP 주소와 포트를 소켓에 할당함.
    - 클라이언트가 데이터를 전송할 수 있도록 주소를 고정시킴.

3. **recvfrom() / sendto()**    
    - 클라이언트로부터 데이터를 수신하거나 전송함.
    - UDP는 비연결형이므로, 각 패킷에 송신자 주소 정보가 함께 전달됨.

4. **close()**    
    - 소켓 종료.

### 🔹 클라이언트(Client)

1. **socket()**    
    - 클라이언트용 UDP 소켓 생성.

2. **sendto() / recvfrom()**    
    - 서버의 주소(IP, 포트)를 지정하여 데이터 전송(`sendto()`).
    - 서버로부터 데이터를 수신(`recvfrom()`).

3. **close()**    
    - 연결 종료.


## `recvfrom()` 함수

UDP에서 데이터를 **수신(receive)** 하는 함수임.

`#include <sys/socket.h> ssize_t recvfrom(int sockfd, void *buffer, size_t length, int flags,                  struct sockaddr *src_addr, socklen_t *addrlen);`

### 주요 매개변수

- **sockfd:** 수신용 소켓 파일 디스크립터    
- **buffer:** 수신한 데이터를 저장할 버퍼
- **length:** 수신할 최대 바이트 크기
- **flags:** 메시지 수신 방식 (일반적으로 0)
- **src_addr:** 송신자(상대방)의 주소 정보를 저장할 구조체 포인터
- **addrlen:** 주소 구조체의 크기를 담는 변수

### 반환값

- **성공:** 수신한 바이트 수
- **EOF(연결 종료):** 0
- **오류:** -1

### 예시

`str_len = recvfrom(serv_sock, message, BUF_SIZE, 0,                    (struct sockaddr*)&clnt_adr, &clnt_adr_sz);`

## `sendto()` 함수

UDP에서 데이터를 **전송(send)** 하는 함수임.

`#include <sys/socket.h> ssize_t sendto(int sockfd, const void *buffer, size_t length, int flags,                const struct sockaddr *dest_addr, socklen_t addrlen);`

### 주요 매개변수

- **sockfd:** 송신용 소켓 파일 디스크립터
- **buffer:** 전송할 데이터를 담은 버퍼
- **length:** 전송할 데이터 크기
- **flags:** 메시지 전송 방식 (일반적으로 0)
- **dest_addr:** 데이터를 보낼 대상의 주소 구조체 포인터
- **addrlen:** 주소 구조체의 크기

### 반환값

- **성공:** 전송한 바이트 수
- **오류:** -1
  

### 예시

`sendto(serv_sock, message, str_len, 0,        (struct sockaddr*)&clnt_adr, clnt_adr_sz);`



### ⚙️ 요약

|구분|함수명|역할|특징|
|||||
|서버|`socket()`|소켓 생성|UDP 소켓은 연결 과정 없음|
|서버|`bind()`|IP/포트 할당|클라이언트가 접근할 수 있도록 설정|
|양쪽|`recvfrom()`|데이터 수신|송신자 주소 정보 함께 수신|
|양쪽|`sendto()`|데이터 전송|대상 주소를 직접 지정|
|양쪽|`close()`|소켓 종료|리소스 해제|



**정리:**  
UDP는 TCP와 달리 **연결 설정(connect/accept 과정)** 이 없으며,  
데이터 전송 시마다 `sendto()`와 `recvfrom()`을 통해 **주소 정보를 함께 주고받는 구조**를 가진다.


## Connected UDP (연결형 UDP)

### 🔹 `sendto()` 함수의 내부 동작 과정

UDP에서 `sendto()`가 호출될 때 내부적으로 다음 단계가 수행됨.

1. **목적지 IP와 포트 할당 (Allocate)**  
    UDP 소켓에 목적지 주소를 일시적으로 연결함.    
2. **데이터 전송 (Send)**  
    해당 IP와 포트로 데이터를 전송함.
3. **주소 해제 (Deallocate)**  
    데이터 전송 후, 목적지 주소 정보(IP/포트)를 소켓에서 해제함.

### 🔸 Connected UDP란

- **목적지 주소를 미리 지정해 둔 UDP 소켓**을 의미함.  
    즉, 한 번 `connect()`를 호출하여 대상 주소를 고정해 두면,  
    매번 `sendto()` 호출 시 주소를 다시 지정할 필요가 없음.    
- **장점:**
    - 주소 할당/해제 절차를 생략하므로 **성능 향상**
    - `sendto()` / `recvfrom()` 대신 **`write()` / `read()`** 를 사용할 수 있음
- **주의:**
    - 이름은 “Connected UDP”이지만, **TCP처럼 연결형(신뢰성 보장)** 프로토콜이 되는 것은 아님.
    - 여전히 UDP 특유의 **비연결성·비신뢰성**은 유지됨.

### 🔹 Connected UDP 생성 방법

`sock = socket(PF_INET, SOCK_DGRAM, 0); if (sock == -1)     error_handling("socket() error");  memset(&serv_adr, 0, sizeof(serv_adr)); serv_adr.sin_family = AF_INET; serv_adr.sin_addr.s_addr = inet_addr(argv[1]); serv_adr.sin_port = htons(atoi(argv[2]));  connect(sock, (struct sockaddr*)&serv_adr, sizeof(serv_adr));`

- `socket()` : UDP 소켓 생성 (`SOCK_DGRAM`)    
- `connect()` : 특정 서버 주소(IP, 포트)에 UDP 소켓을 논리적으로 연결  
    → 이후에는 `sendto()` 대신 `write()`, `recvfrom()` 대신 `read()` 사용 가능

### ⚙️ 요약 비교

| 구분       | 일반 UDP                   | Connected UDP        |
| -------- | ------------------------ | -------------------- |
| 주소 지정    | 매번 `sendto()`에서 명시       | 한 번만 `connect()`로 설정 |
| 송수신 함수   | `sendto()`, `recvfrom()` | `write()`, `read()`  |
| 주소 할당/해제 | 매 전송마다 수행                | 최초 한 번만 수행           |
| 연결 여부    | 비연결형                     | 논리적 연결(물리적 연결 아님)    |
| 신뢰성      | 없음                       | 없음 (UDP 특성 동일)       |

**정리:**  
Connected UDP는 TCP처럼 실제 연결을 유지하지는 않지만,  
하나의 대상에만 지속적으로 통신할 때 **주소 설정을 단순화하고 시스템 오버헤드를 줄이는 방법**이다.


### 🔹 `SO_RCVBUF`, `SO_SNDBUF` — 송수신 버퍼 크기 설정

- **SO_RCVBUF:** 수신 버퍼의 최대 크기를 설정하거나 조회함.
- **SO_SNDBUF:** 송신 버퍼의 최대 크기를 설정하거나 조회함.

#### ✅ 사용 예시 (`set_buf.c`)

`int sock; int snd_buf = 1024 * 3, rcv_buf = 1024 * 3; int state; socklen_t len;  sock = socket(PF_INET, SOCK_STREAM, 0);  state = setsockopt(sock, SOL_SOCKET, SO_RCVBUF, (void*)&rcv_buf, sizeof(rcv_buf)); state = setsockopt(sock, SOL_SOCKET, SO_SNDBUF, (void*)&snd_buf, sizeof(snd_buf));  len = sizeof(snd_buf); state = getsockopt(sock, SOL_SOCKET, SO_SNDBUF, (void*)&snd_buf, &len);  len = sizeof(rcv_buf); state = getsockopt(sock, SOL_SOCKET, SO_RCVBUF, (void*)&rcv_buf, &len);  printf("Input buffer size: %d\n", rcv_buf); printf("Output buffer size: %d\n", snd_buf);`

#### ⚠️ 주의

- 커널이 실제로 설정하는 값은 사용자가 지정한 값과 다를 수 있음.
- 보통 **커널이 내부적으로 지정값의 약 2배 크기로 조정함.**

### ⚙️ 요약 비교

|옵션|기능|주요 사용 목적|
|---|---|---|
|`SO_REUSEADDR`|포트 재사용 허용|TIME_WAIT 상태나 다중 IP 환경에서 즉시 바인딩 가능|
|`SO_RCVBUF`|수신 버퍼 크기 설정|네트워크 병목 방지, 대용량 데이터 처리|
|`SO_SNDBUF`|송신 버퍼 크기 설정|대역폭 확보, 송신 성능 향상|


**정리:**  
`SO_REUSEADDR`은 서버 재시작 시 포트를 즉시 재사용할 수 있게 하는 중요한 옵션이며,  
`SO_RCVBUF`과 `SO_SNDBUF`은 TCP 버퍼 크기를 조정해 통신 효율을 최적화하는 데 사용된다.


## `SOL_SOCKET`: 소켓 옵션 설정

### 🔹 `SO_REUSEADDR` — 포트 재사용 옵션

- 이미 사용 중이거나 **TIME_WAIT** 상태에 있는 포트를 재사용할 수 있도록 허용함.
- 서버가 재시작될 때, 이전 연결의 포트가 아직 완전히 해제되지 않아도 **즉시 bind() 가능**하게 해줌.

#### ✅ 사용 예시

`int option; socklen_t optlen;  optlen = sizeof(option); option = TRUE; setsockopt(serv_sock, SOL_SOCKET, SO_REUSEADDR, &option, optlen);`

#### 📘 필요한 경우

1. **TIME_WAIT 상태의 서버**
    - 서버가 종료된 직후 같은 포트를 다시 열 때, 포트가 TIME_WAIT 상태라면 기본적으로 바인딩이 불가함.
    - `SO_REUSEADDR`을 설정하면 해당 포트를 즉시 재사용할 수 있음.

2. **멀티홈(Multi-home) 서버**    
    - 여러 네트워크 인터페이스(IP)를 가진 서버가 동일 포트를 공유해야 하는 경우 사용됨.

#### ⚙️ 동작 개요

- TCP 연결 종료 시, 커널은 일정 시간 동안 TIME_WAIT 상태를 유지함.
- 이 상태는 잔여 패킷 처리를 위해 필요하지만, `SO_REUSEADDR`을 설정하면 대기 없이 동일 포트를 재활용 가능함.
- 
>>>>>>> origin/main
