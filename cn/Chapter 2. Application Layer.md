
# Application Layer: Overview

## 목표

- 애플리케이션 계층 프로토콜의 개념적 **및** 구현적 측면 학습
    - 전송 계층 서비스 모델
    - 클라이언트-서버 패러다임
    - 피어-투-피어 패러다임
- 널리 사용되는 애플리케이션 계층 프로토콜을 통해 학습
    - HTTP
    - SMTP, IMAP
    - DNS

# Creating a Network App (네트워크 앱 만들기)

## 작성해야 하는 프로그램

- (서로 다른) 종단 시스템(end systems)에서 실행
- 네트워크를 통해 통신
- 예: 웹 서버 소프트웨어가 브라우저 소프트웨어와 통신

## 작성할 필요 없는 부분

- 네트워크 코어 장치(network-core devices)를 위한 소프트웨어는 작성할 필요 없음
    - 네트워크 코어 장치는 사용자 애플리케이션을 실행하지 않음
    - 애플리케이션은 종단 시스템(end systems)에서 실행됨
    - 이를 통해 빠른 애플리케이션 개발 및 배포 가능

📌 요약: **네트워크 애플리케이션은 종단 시스템에서 실행되며, 네트워크 코어 장치에는 직접 작성할 필요가 없다.**


# Client-Server Paradigm (클라이언트-서버 패러다임)

## 서버 (server)

- 항상 켜져 있는 호스트 (always-on host)
- 고정 IP 주소 보유 (permanent IP address)
- 확장성을 위해 데이터 센터에 위치하는 경우 많음
## 클라이언트 (clients)

- 서버와 연결하여 통신
- 간헐적으로 연결될 수 있음
- 동적 IP 주소를 가질 수 있음
- 서로 직접 통신하지 않음
- 예시: **HTTP, IMAP, FTP**

📌 요약: **클라이언트는 서버를 통해 통신하며, 서버는 항상 켜져 있고 고정된 주소를 가진다.**


# Peer-to-Peer Architecture (P2P 아키텍처)

- **항상 켜져 있는 서버 없음 (no always-on server)**
- 임의의 종단 시스템이 직접 통신 가능
- 피어는 다른 피어에게 서비스를 요청하고, 동시에 다른 피어에게 서비스를 제공
    - _자체 확장성 (self scalability)_: 새로운 피어가 참여하면 새로운 서비스 용량과 동시에 새로운 서비스 수요가 함께 추가됨
- 피어는 간헐적으로 연결되며 IP 주소가 바뀔 수 있음
    - 관리가 복잡해짐 (complex management)
- **예시**: P2P 파일 공유, 블록체인

📌 요약: **P2P는 중앙 서버 없이 피어들끼리 직접 통신하고 자율적으로 확장되지만, 관리가 복잡하다.**


# Processes Communicating (프로세스 간 통신)

## 프로세스 (process)

- 호스트 내에서 실행되는 프로그램
## 동일 호스트 내 통신

- 두 프로세스는 **프로세스 간 통신(inter-process communication, IPC)**을 통해 통신
- IPC는 운영체제(OS)에 의해 정의됨
## 다른 호스트 간 통신

- 서로 다른 호스트에 있는 프로세스는 **메시지 교환(messages)**을 통해 통신
## 클라이언트-서버 프로세스

- **클라이언트 프로세스 (client process)**: 통신을 시작하는 프로세스
- **서버 프로세스 (server process)**: 접속을 기다리는 프로세스
## 참고

- P2P 아키텍처를 사용하는 애플리케이션도 클라이언트 프로세스와 서버 프로세스를 모두 가짐

📌 요약: **프로세스는 같은 호스트에서는 IPC로, 다른 호스트에서는 메시지 교환으로 통신한다. P2P 구조도 결국 클라이언트·서버 프로세스로 동작한다.**


# Sockets (소켓)

## 정의

- 프로세스는 소켓을 통해 메시지를 송수신함
## 소켓의 비유

- 소켓은 **문(door)**과 유사
    - 송신 프로세스는 메시지를 문 밖으로 내보냄
    - 송신 프로세스는 문 건너편의 **전송 계층 인프라**에 의존하여 수신 프로세스의 소켓까지 메시지를 전달
- 두 개의 소켓이 필요 (각각 송신 측과 수신 측)
## 제어 권한

- **애플리케이션 개발자**: 애플리케이션 계층에서 소켓 제어
- **운영체제(OS)**: 전송, 네트워크, 링크, 물리 계층 제어

📌 요약: **소켓은 프로세스가 네트워크를 통해 메시지를 주고받는 출입구 역할을 하며, 애플리케이션 개발자는 소켓을 제어하지만 실제 전달은 OS의 전송 인프라에 의존한다.**


# Addressing Processes (프로세스 주소 지정)

## 메시지 수신을 위한 조건

- 프로세스는 반드시 **식별자(identifier)**를 가져야 함
- 호스트 장치는 고유한 **32비트 IP 주소**를 가짐
## 질문 (Q&A)

- Q: 프로세스가 실행되는 호스트의 IP 주소만으로 해당 프로세스를 식별할 수 있는가?
- A: **아니오.** 하나의 호스트에서 여러 프로세스가 동시에 실행될 수 있음
## 식별자(identifier)

- **IP 주소 + 포트 번호**로 구성됨
- 예시 포트 번호:
    - HTTP 서버: `80`
    - 메일 서버: `25`
## 예시

- 웹 서버 `gaia.cs.umass.edu`로 HTTP 메시지를 보낼 경우:
    - IP 주소: `128.119.245.12`
    - 포트 번호: `80`

📌 요약: **프로세스를 구분하기 위해서는 IP 주소와 포트 번호가 함께 필요하다.**


# An Application-Layer Protocol Defines (애플리케이션 계층 프로토콜이 정의하는 것)

## 프로토콜이 정의하는 요소

- **메시지 유형 (types of messages exchanged)**    
    - 예: 요청(request), 응답(response)

- **메시지 구문 (message syntax)**
    - 메시지에 포함된 필드와 그 구분 방법

- **메시지 의미 (message semantics)**    
    - 각 필드의 정보 의미

- **규칙 (rules)**    
    - 언제, 어떻게 프로세스가 메시지를 보내고 응답하는지

## 프로토콜 유형

- **오픈 프로토콜 (open protocols)**    
    - RFC에 정의되어 있어 누구나 접근 가능
    - 상호 운용성(interoperability) 보장
    - 예: HTTP, SMTP

- **독점 프로토콜 (proprietary protocols)**    
    - 특정 기업/제품 전용
    - 예: Skype


📌 요약: **애플리케이션 계층 프로토콜은 메시지의 형식, 의미, 규칙을 정의하며 오픈 또는 독점 방식으로 나뉜다.**

# What Transport Service Does an App Need?

## Data Integrity (데이터 무결성)

- 일부 앱(예: 파일 전송, 웹 거래)은 **100% 신뢰할 수 있는 데이터 전송** 필요
- 다른 앱(예: 오디오)은 일부 손실을 허용 가능
## Timing (타이밍)

- 일부 앱(예: 인터넷 전화, 인터랙티브 게임)은 **낮은 지연 시간**이 필요
## Throughput (처리량)

- 일부 앱(예: 멀티미디어)은 **최소한의 처리량 보장** 필요
- 다른 앱(“elastic apps”)은 가용한 처리량을 그대로 활용
## Security (보안)

- 암호화, 데이터 무결성 보장 등 필요

📌 요약: **애플리케이션의 특성에 따라 전송 계층이 보장해야 하는 요소(무결성, 지연, 처리량, 보안)가 달라진다.**


![[Pasted image 20250915105132.png]]

# Internet Transport Protocols Services

## TCP Service

- **신뢰성 있는 전송 (reliable transport)**: 송신 프로세스와 수신 프로세스 간 데이터 전송 보장   
- **흐름 제어 (flow control)**: 송신자가 수신자를 압도하지 않도록 조절
- **혼잡 제어 (congestion control)**: 네트워크 과부하 시 송신자 속도 제한
- **제공하지 않는 것**: 타이밍, 최소 처리량 보장, 보안
- **연결 지향적 (connection-oriented)**: 클라이언트와 서버 간 연결 설정 필요
## UDP Service

- **신뢰성 없는 전송 (unreliable data transfer)**: 송신-수신 간 전송 보장 없음
- **제공하지 않는 것**:
    - 신뢰성 (reliability)
    - 흐름 제어 (flow control)
    - 혼잡 제어 (congestion control)
    - 타이밍 (timing)
    - 처리량 보장 (throughput guarantee)
    - 보안 (security)
    - 연결 설정 (connection setup)
## 질문
- Q: 그런데 왜 굳이 UDP가 있는가?

📌 요약:
- **TCP**는 신뢰성과 제어 기능을 제공하지만, 무겁고 연결 설정이 필요함. 
- **UDP**는 단순하고 가볍지만 신뢰성을 제공하지 않음.

![[Pasted image 20250915105313.png]]

# Web and HTTP

## 웹 페이지 구성

- 웹 페이지는 여러 **객체(objects)**로 이루어지며, 각 객체는 서로 다른 웹 서버에 저장될 수 있음    
- 객체(object)의 예시:
    - HTML 파일
    - JPEG 이미지
    - Java 애플릿
    - 오디오 파일 등

## 웹 페이지 구조

- 하나의 웹 페이지는 **기본 HTML 파일(base HTML-file)**로 구성됨
- 기본 HTML 파일은 **여러 참조 객체(referenced objects)**를 포함
- 각 참조 객체는 **URL**을 통해 접근 가능
### URL 예시

`www.someschool.edu/someDept/pic.gif`

- Host name: `www.someschool.edu`
- Path name: `/someDept/pic.gif`

📌 요약: **웹 페이지 = 기본 HTML + 여러 참조 객체(이미지, 오디오 등), 각각은 URL로 식별됨.**


# HTTP Overview

## HTTP: Hypertext Transfer Protocol

- 웹의 **애플리케이션 계층 프로토콜**
- **클라이언트/서버 모델**
    - **Client**:
        - 브라우저(예: Firefox, Safari)
        - HTTP 프로토콜을 사용하여 웹 객체 요청 및 수신
        - 수신한 객체를 화면에 표시
    - **Server**:
        - 웹 서버(예: Apache Web Server)
        - 클라이언트 요청에 따라 HTTP 프로토콜을 사용해 웹 객체를 전송

# HTTP Overview (continued)

## HTTP uses TCP

- 클라이언트가 서버(포트 80)에 **TCP 연결**을 생성 (소켓 생성)    
- 서버는 클라이언트의 TCP 연결을 수락
- 브라우저(HTTP 클라이언트)와 웹 서버(HTTP 서버) 간에 **HTTP 메시지 교환**
- 데이터 교환이 끝나면 TCP 연결 종료

## HTTP is Stateless

- 서버는 과거 클라이언트 요청에 대한 **상태 정보(state)를 저장하지 않음**
- 장점: 단순하고 효율적
- 단점: 상태 정보를 유지하지 않음

### Aside: 상태 유지 프로토콜은 복잡함

- 과거 기록(state)을 관리해야 함
- 서버나 클라이언트가 다운될 경우 상태 정보 불일치 문제가 발생할 수 있음
- 불일치를 조정해야 하는 부담이 있음

📌 요약:
- **HTTP는 TCP 기반, 클라이언트-서버 구조, 상태를 저장하지 않는(stateless) 프로토콜**이다.


# HTTP Connections: Two Types
## Non-persistent HTTP (비지속 연결)

1. TCP 연결 생성
2. 한 개의 객체만 전송 가능
3. TCP 연결 종료

- 여러 객체 다운로드 시, **객체마다 별도의 연결 필요**

거의 안씀 

## Persistent HTTP (지속 연결)

- 하나의 TCP 연결을 서버와 맺음
- 클라이언트와 서버 간 **하나의 TCP 연결로 여러 객체 전송 가능**
- 모든 객체 전송 후 TCP 연결 종료

📌 요약:
- **Non-persistent HTTP** → 객체마다 새로운 연결 생성 → 비효율적
- **Persistent HTTP** → 하나의 연결로 여러 객체 전송 → 효율적

# Non-persistent HTTP: Example

(비지속 HTTP 동작 예시)

사용자가 입력한 URL:

`www.someSchool.edu/someDepartment/home.index`

## 동작 과정

1a. HTTP 클라이언트가 **TCP 연결 요청**을 시작

- 대상: `www.someSchool.edu`
- 포트: `80`

1b. HTTP 서버(`www.someSchool.edu`)는 포트 80에서 대기하다가 연결을 **수락(accept)**하고 클라이언트에 알림

2. HTTP 클라이언트가 **HTTP 요청 메시지(request message)**를 TCP 연결 소켓에 전송

- 메시지에는 URL 포함
- 예: 클라이언트가 `someDepartment/home.index` 객체를 원한다는 정보

3. HTTP 서버는 요청 메시지를 받고, **HTTP 응답 메시지(response message)**를 생성

- 요청된 객체를 포함  
- 소켓을 통해 클라이언트로 전송

📌 요약:  
비지속 HTTP에서는 **한 객체를 요청할 때마다 새로운 TCP 연결을 생성하고 종료**한다.

4. **HTTP 서버가 TCP 연결을 종료**
5. **HTTP 클라이언트가 응답 메시지를 수신**

- 응답에는 HTML 파일 포함
- 클라이언트는 HTML을 표시
- HTML을 파싱하여 10개의 JPEG 객체 참조 발견

6. **JPEG 객체 요청을 위해 1~5단계를 반복**

- 각 JPEG 객체마다 새로운 TCP 연결 생성 및 종료

📌 요약:

- 비지속 HTTP에서는 **하나의 객체 전송마다 새로운 TCP 연결이 필요**   
- HTML 내부에 여러 참조 객체가 있을 경우, **각 객체마다 반복 연결**해야 함 → 비효율적

# Non-persistent HTTP: Response Time

(비지속 HTTP 응답 시간)
## RTT (Round Trip Time) 정의

- 작은 패킷이 **클라이언트 → 서버 → 클라이언트**로 왕복하는 데 걸리는 시간
## HTTP 응답 시간 (객체당)

1. **1 RTT**: TCP 연결을 초기화하는 데 필요
2. **1 RTT**: HTTP 요청 전송 및 HTTP 응답의 첫 바이트 수신
3. 객체(파일) 전송 시간

➡️ 따라서,  
**Non-persistent HTTP 응답 시간 = 2 RTT + 파일 전송 시간**

📌 요약:
- 비지속 HTTP에서는 객체 하나를 전송할 때 **2번의 RTT + 전송 시간**이 필요 
- 여러 객체를 요청할 경우, 객체마다 이 과정이 반복되므로 비효율적


# Persistent HTTP (HTTP 1.1)

## Non-persistent HTTP의 문제점

- 객체당 **2 RTT 필요**
- 각 TCP 연결마다 **운영체제(OS) 오버헤드** 발생
- 브라우저는 참조된 객체를 가져오기 위해 종종 **여러 병렬 TCP 연결**을 열어야 함
## Persistent HTTP (HTTP 1.1)의 특징

- 서버가 응답을 보낸 후에도 **TCP 연결을 유지**
- 같은 클라이언트/서버 간 **여러 HTTP 메시지를 단일 연결에서 교환**
- 클라이언트는 참조 객체를 발견하는 즉시 요청 가능
- 모든 참조 객체 전송에 **최소 1 RTT만 필요** → 응답 시간이 절반으로 줄어듦

📌 요약:
- **Non-persistent HTTP**: 객체마다 새 연결 필요 → 비효율적  
- **Persistent HTTP (1.1)**: 단일 연결 유지 → RTT 감소, 성능 향상

## Exercise

가정:

- 클라이언트와 웹 서버 간의 RTT는 **X**입니다.
- HTML 파일은 같은 웹 서버에 있는 아주 작은 객체 8개를 참조합니다.
- 전송 시간은 무시합니다.

질문: 다음 경우 각각 시간은 얼마나 걸립니까?

a. 비지속적 HTTP(non-persistent HTTP), 병렬 TCP 연결 없음  
b. 비지속적 HTTP(non-persistent HTTP), 브라우저가 5개 병렬 연결로 동작  
c. 지속적 연결(persistent connection) + 파이프라이닝 (HTTP 기본 모드)  
d. 지속적 연결(persistent connection), 파이프라이닝 없음, 병렬 없음

- **a. 비지속적 + 병렬 없음**
    - HTML 파일: 2X
    - 객체 8개: 각각 2X씩 → 16X
    - 합계: **18X**
- **b. 비지속적 + 5개 병렬**
    - HTML 파일: 2X
    - 첫 5개 객체: 동시에 요청 → 2X
    - 나머지 3개 객체: 동시에 요청 → 2X
    - 합계: **6X**
- **c. 지속적 + 파이프라이닝**
    - HTML 파일: 2X
    - 객체 8개를 파이프라이닝 요청 → 1X
    - 합계: **3X**
- **d. 지속적 + 파이프라이닝 없음**
    - HTML 파일: 2X
    - 객체 8개: 각각 1X씩 → 8X
    - 합계: **10X**

## HTTP 요청 메시지 (HTTP Request Message)

### HTTP 메시지의 두 종류
- **Request (요청)**
- **Response (응답)**    

### HTTP 요청 메시지 특징

- **ASCII 기반 (사람이 읽을 수 있는 텍스트 형식)**    

### 구조

1. **Request line (요청 라인)**
    - 예: `GET /index.html HTTP/1.1`        
    - 요청 메서드(GET, POST, HEAD 등) + URL + HTTP 버전

2. **Header lines (헤더 라인)**
    - 예시:
        Host: www-net.cs.umass.edu
		User-Agent: Firefox/3.6.10
		Accept: text/html,application/xhtml+xml
		Accept-Language: en-us,en;q=0.5
		Accept-Encoding: gzip,deflate
		Accept-Charset: ISO-8859-1,utf-8;q=0.7
		Keep-Alive: 115
		Connection: keep-alive
3. **빈 줄 (Carriage return + Line feed = `\r\n`)**    
    - 헤더 부분이 끝났음을 알림

📌 요약:  
HTTP 요청 메시지는 **요청 라인 → 헤더 라인 → 빈 줄** 순으로 구성되며, 각 줄은 `\r\n`으로 끝납니다.

![[Pasted image 20250918102850.png]]

## 다른 HTTP 요청 메서드 (Other HTTP Request Messages)

### 1. **GET 메서드**

- 리소스를 가져옴 (retrieve resource)
- 사용자 데이터를 URL에 포함시킴 (`?` 뒤에 붙음)
- 예시:
    `www.somesite.com/animalsearch?monkeys&banana`

### 2. **HEAD 메서드**

- 리소스 본문은 가져오지 않고 **헤더 정보만 요청**    
- 만약 해당 URL을 GET으로 요청했다면 반환될 헤더만 확인 가능

### 3. **POST 메서드**

- 주로 웹 페이지의 **폼 입력(form input)**에 사용
- 클라이언트에서 서버로 데이터를 **HTTP 요청 메시지의 본문(body)**에 담아 전송

### 4. **PUT 메서드**

- 새로운 파일(객체)을 서버에 업로드   
- 지정된 URL에 기존 파일이 있으면, 본문(body)에 담긴 내용으로 완전히 교체

📌 요약

- **GET**: 리소스 요청 (데이터 URL에 포함)   
- **HEAD**: 헤더만 요청
- **POST**: 데이터 전송 (본문에 포함)
- **PUT**: 파일 업로드 및 교체


![[Pasted image 20250918103346.png]]


## HTTP 응답 상태 코드 (HTTP Response Status Codes)

- 상태 코드는 **서버 → 클라이언트 응답 메시지의 첫 줄**에 나타남
### 주요 예시

- **200 OK**
    - 요청 성공, 요청한 객체가 응답 메시지에 포함됨
- **301 Moved Permanently**
    - 요청한 객체가 다른 위치로 이동됨
    - 새로운 위치는 응답의 `Location:` 헤더에 명시됨
- **400 Bad Request**
    - 요청 메시지를 서버가 이해할 수 없음
- **404 Not Found**
    - 요청한 문서를 서버에서 찾을 수 없음
- **505 HTTP Version Not Supported**
    - 요청에 사용된 HTTP 버전을 서버가 지원하지 않음        

📌 요약

- **200**: 성공
- **301**: 영구 이동
- **400**: 잘못된 요청
- **404**: 없음
- **505**: HTTP 버전 미지원


## 사용자/서버 상태 유지: Cookies

### HTTP의 특징

- **HTTP GET/Response 상호작용은 stateless (무상태)**
- 즉, 여러 단계의 HTTP 메시지 교환을 통한 "트랜잭션" 개념이 없음    

### Stateless의 의미

- 클라이언트/서버가 다단계 교환의 **state(상태)**를 추적할 필요 없음
- 모든 HTTP 요청은 **서로 독립적**임
- 부분적으로만 완료된 거래를 **복구할 필요 없음**    

### Stateful Protocol과 비교

- 상태 있는 프로토콜(stateful protocol)에서는
    - 클라이언트가 X라는 데이터를 잠금(lock) → 수정(update) → 다시 잠금 해제(unlock)
    - 이 과정에서 상태 변화가 추적됨
- 만약 네트워크 연결이 끊기거나 클라이언트가 중간에 실패하면(`t'` 지점),
    - 트랜잭션이 불완전하게 끝날 수 있음   



📌 요약
- HTTP는 기본적으로 **stateless** → 서버는 요청 간 상태를 기억하지 않음
- 사용자 인증, 장바구니, 세션 유지 등을 위해 **쿠키(cookies)** 같은 별도 메커니즘이 필요


## 사용자/서버 상태 유지: Cookies

웹사이트와 클라이언트 브라우저는 **쿠키(cookies)**를 사용하여 트랜잭션 간 일부 상태를 유지함.

### 쿠키의 4가지 구성 요소

1. **HTTP Response 메시지의 Cookie 헤더 라인**
    - 서버가 클라이언트에 쿠키를 전달

2. **다음 HTTP Request 메시지의 Cookie 헤더 라인**    
    - 브라우저가 저장한 쿠키를 서버에 다시 전송

3. **사용자 PC(호스트)에 저장된 쿠키 파일**    
    - 브라우저가 관리

4. **웹사이트 백엔드 데이터베이스**    
    - 쿠키 ID와 사용자 관련 데이터를 관리

### 예시 (e-commerce 사이트 방문)

- **처음 방문**   
    - Susan이 브라우저로 특정 쇼핑몰 사이트에 접속
    - 사이트는 **고유 ID(쿠키)** 생성
    - 백엔드 데이터베이스에 ID 저장

- **다음 요청부터**    
    - Susan의 HTTP 요청에는 쿠키 ID가 포함됨
    - 서버는 이를 이용해 **사용자를 식별** 가능

📌 요약

- 쿠키는 **클라이언트–서버 간 상태 정보 유지 도구**   
- 서버는 **쿠키 ID ↔ DB**를 통해 사용자를 추적 및 식별
- 브라우저는 **쿠키 파일**을 관리하며 요청 시 포함

![[Pasted image 20250918105520.png]]


## HTTP Cookies: Comments

### 쿠키의 활용 (What cookies can be used for)

- **Authorization (인증)**
- **Shopping carts (장바구니)**
- **Recommendations (추천 시스템)**
- **User session state (사용자 세션 상태, 예: 웹 이메일 로그인 유지)**

### 도전 과제: 상태 유지 방법 (Challenge: How to keep state)

- 프로토콜 종단(endpoint)에서 **여러 트랜잭션 동안 상태를 유지**해야 함    
- 쿠키를 통해 HTTP 메시지가 상태 정보를 함께 전달

### 쿠키와 프라이버시 (Cookies and Privacy)

- 쿠키는 웹사이트가 사용자에 대해 많은 것을 **학습(learn)**할 수 있도록 함    
- 제3자 지속 쿠키(third-party persistent cookies, 추적 쿠키)는
    - 동일한 ID(쿠키 값)를 여러 웹사이트에 걸쳐 추적 가능
    - 즉, **사용자 활동이 사이트 간에 공유**될 수 있음

📌 요약

- 쿠키는 인증·세션·장바구니 등 **편리한 기능** 제공   
- 하지만 추적 쿠키는 **개인정보 침해** 우려 존재


## Web Caches (Proxy Servers)

### 목표 (Goal)

- **원 서버(origin server)에 직접 접근하지 않고 클라이언트 요청을 처리하는 것**    

### 동작 방식

1. 사용자가 브라우저를 **웹 캐시(Web cache)**를 사용하도록 설정    
2. 브라우저는 모든 HTTP 요청을 캐시에 전송

- **if (객체가 캐시에 존재한다면)**  
    → 캐시가 해당 객체를 클라이언트에 반환

- **else (캐시에 없다면)**  
    → 캐시가 원 서버(origin server)에 요청  
    → 받은 객체를 저장(cache) 후 클라이언트에 반환    

### 요약

- 웹 캐시는 **프록시 서버(proxy server)** 역할 수행   
- 자주 요청되는 객체를 캐시에 보관하여
    - **응답 시간 단축**
    - **원 서버 부하 감소**


