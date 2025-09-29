
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


## Web Caches (Proxy Servers)

### 웹 캐시의 역할

- 웹 캐시는 **클라이언트이자 서버**로 동작
    - 클라이언트 입장: 원 서버(origin server)에 요청 전달
    - 서버 입장: 원래 요청한 클라이언트에게 응답 제공

- 일반적으로 캐시는 **ISP(인터넷 서비스 제공자)**가 설치    
    - 예: 대학, 기업, 가정용 ISP

### 왜 웹 캐싱을 사용하는가? (Why Web Caching?)

- **클라이언트 응답 시간 단축**   
    - 캐시가 클라이언트에 더 가까움

- **기관의 접속 링크 트래픽 감소**    
- **인터넷 전반에 캐시가 밀집되어 있음**
    - 자원이 부족한 콘텐츠 제공자도 더 효과적으로 콘텐츠 전달 가능

📌 요약

- 캐시는 요청을 빠르게 처리하고, 네트워크 트래픽을 줄이며, 인터넷 성능을 향상시킴   
- ISP, 기업, 학교 등에서 적극 활용됨

## Caching Example

### 시나리오 (Scenario)

- 접속 링크 속도: **1.54 Mbps**   
- RTT (기관 라우터 ↔ 서버): **2초**
- 웹 객체 크기: **100K bits**
- 브라우저 → 원 서버 평균 요청률: **15회/초**
    - 평균 데이터 전송률: **1.50 Mbps**

초당 데이터량=100,000(100k)×15=1,500,000 bits/초
1 Mbps = 1,000,000 bits/sec

1,500,000/1,000,000=1.5 Mbps

### 성능 (Performance)

- LAN 이용률: **0.0015** (거의 영향 없음)    
- 접속 링크 이용률: **0.97 (97%)**
    - **문제:** 높은 이용률에서 큰 지연 발생

- End-to-End 지연 = 인터넷 지연 + 접속 링크 지연 + LAN 지연    
    - ≈ **2초 + 수분 + 마이크로초(us)**

### 요약

- LAN은 충분히 빠르지만(1 Gbps),    
- **병목 현상**은 1.54 Mbps의 **Access Link**에서 발생
- 높은 활용률(97%) 때문에 **지연 시간 심각**


## Caching Example: Buy a Faster Access Link

### 시나리오 (Scenario)

- 접속 링크 속도: **154 Mbps** (업그레이드 전: 1.54 Mbps)
- RTT (기관 라우터 ↔ 서버): **2초**
- 웹 객체 크기: **100K bits**
- 브라우저 → 원 서버 평균 요청률: **15회/초**
    - 평균 데이터 전송률: **1.50 Mbps**

### 성능 (Performance)

- LAN 이용률: **0.0015** (거의 영향 없음)   
- 접속 링크 이용률: **0.0097 (0.97%)**
    - 업그레이드 전: 97% → 병목
    - 업그레이드 후: 0.97% → 충분한 대역폭
- End-to-End 지연
    - = 인터넷 지연 + 접속 링크 지연 + LAN 지연
    - ≈ **2초 + 밀리초(ms) 단위 지연**

### 비용 (Cost)

- 빠른 접속 링크 구입은 **매우 비쌈**   

📌 요약

- **문제 해결 방법:** 접속 링크 속도를 늘리면 병목 해소 가능   
- **단점:** 비용이 크므로 모든 기관에서 적용하기 어려움


## Caching Example: Install a Web Cache

### 시나리오 (Scenario)

- 접속 링크 속도: **1.54 Mbps**
- RTT (기관 라우터 ↔ 서버): **2초**
- 웹 객체 크기: **100K bits**
- 브라우저 → 원 서버 평균 요청률: **15회/초**
    - 평균 데이터 전송률: **1.50 Mbps**

### 성능 지표 (Performance)

- **LAN Utilization = ?**   
    - 대부분의 요청이 **Local Cache**에서 처리되므로 LAN 이용률은 약간 증가
- **Access Link Utilization = ?**
    - 원 서버(origin server)로 가는 요청이 줄어듦 → Access link 이용률 크게 감소
- **평균 End-to-End Delay = ?**
    - 캐시 hit 시: LAN 지연(매우 작음, ms 수준)
    - 캐시 miss 시: 기존과 동일 (2초 + 전송 지연)
    - 따라서 전체 평균 지연은 크게 감소

### 비용 (Cost)

- **웹 캐시 설치 비용은 저렴함**   
- 빠른 링크 구매(비싸고 지속적 비용)보다 효율적

📌 요약

- **웹 캐시의 장점:**   
    - 트래픽 감소 → Access link 부하 해소
    - 평균 응답 시간 단축
    - 비용 절감

- **웹 캐시와 빠른 링크 업그레이드 비교**    
    - 빠른 링크: 고비용, 단순 대역폭 증가
    - 웹 캐시: 저비용, 효율적 자원 활용


## Caching Example: Install a Web Cache (with calculation)

### 가정

- 캐시 히트율(Cache hit rate) = **0.4 (40%)**
    - 40% 요청은 **캐시에서 처리**
    - 60% 요청은 **원 서버(origin)에서 처리**

### Access Link Utilization 계산

- 60% 요청이 Access Link 사용   
- 데이터 전송률 =
    0.6×1.50 Mbps=0.9 Mbps
- 이용률(Utilization) =
    0.9/1.54≈0.58

### 평균 End-to-End Delay 계산

- 평균 지연 =
    0.6×(origin server 지연)+0.4×(cache 지연)
- Origin server 지연 ≈ 2.01초
- Cache 지연 ≈ 수 밀리초(ms)
=0.6×2.01 sec+0.4×( ms)≈1.2 sec

### 결과

- **Access Link Utilization:** 0.58 (기존 0.97 → 개선됨)
- **Average End-to-End Delay:** ≈ 1.2초 (기존 2초+ → 크게 단축)
- **비용:** 캐시 설치는 링크 업그레이드보다 훨씬 저렴

📌 결론

- **웹 캐시**는 고비용의 링크 업그레이드보다    
    - **낮은 비용**으로
    - **낮은 지연 시간**을 제공


![[Pasted image 20250922105710.png]]


![[Pasted image 20250922105754.png]]


## Conditional GET

### Goal

- 캐시에 있는 버전이 최신이면 **객체를 다시 전송하지 않음**
- 장점:
    - 객체 전송 지연 없음
    - 링크 사용량 감소

### 동작 방식

1. **클라이언트(캐시 포함)**    
    - HTTP 요청 시 `If-Modified-Since: <date>` 헤더 포함
    - `<date>`는 캐시에 저장된 복사본의 날짜

2. **서버**    
    - 캐시된 복사본이 최신이면:  
        → `HTTP/1.0 304 Not Modified` 응답 (데이터 전송 안 함)
    - 캐시된 복사본이 오래되었으면:  
        → `HTTP/1.0 200 OK` + 객체 데이터 전송

### 요약

- **If-Modified-Since** 헤더는 클라이언트 캐시가 최신인지 확인 요청    
- 서버는 **304 Not Modified** 또는 **200 OK + data**로 응답
- 효율적인 대역폭 사용과 빠른 응답을 가능하게 함


## HTTP/2

### Key Goal

- **여러 객체를 동시에 요청할 때 지연 감소**    

### HTTP/1.1 특징

- **여러 개의 GET 요청을 단일 TCP 연결에서 파이프라이닝(pipelining) 지원**    
- 서버는 요청을 **순서대로(in-order)** 처리
    - FCFS (First-Come-First-Served) 방식

### 문제점 (HTTP/1.1의 한계)

1. **Head-of-Line (HOL) Blocking**    
    - 큰 객체 뒤에 작은 객체가 있어도 반드시 순서를 기다려야 함

2. **손실 복구 문제**    
    - TCP 세그먼트 손실 시 재전송 필요 → 모든 객체 전송이 지연됨

📌 요약

- HTTP/1.1은 파이프라이닝으로 성능을 개선했지만, **순차 처리(in-order)** 특성 때문에 HOL blocking과 손실 복구 지연 문제가 발생    
- HTTP/2는 이 문제를 해결하기 위해 등장


### 주요 특징 (HTTP/2)

- **기본 메서드, 상태 코드, 대부분의 헤더 필드**는 HTTP/1.1과 동일
- 요청된 객체의 **전송 순서를 클라이언트가 지정한 우선순위**에 따라 조정 가능
    - 더 이상 단순 **FCFS(First-Come-First-Served)** 아님

- 서버가 클라이언트가 요청하지 않은 객체도 **push** 가능    
- 객체를 **프레임 단위로 분할**하여 전송
    - 이를 스케줄링해서 **HOL(Head-of-Line) Blocking** 문제 완화

📌 요약

- HTTP/2는 **멀티플렉싱(multiplexing)과 프레임화**를 통해 객체를 유연하게 전송    
- HOL blocking 문제를 줄이고, 응답 지연을 크게 개선
- 서버 push로 클라이언트 체감 속도 향상


## HTTP/2: Mitigating HOL Blocking

### 문제 (HTTP/1.1)

- 클라이언트가 여러 객체 요청:
    - 큰 객체 O1O_1O1​ (예: 비디오 파일)
    - 작은 객체 O2,O3,O4O_2, O_3, O_4O2​,O3​,O4​

- **HTTP/1.1 특징**: 요청 순서대로(in-order) 객체 전송    
- 결과:
    - O2,O3,O4O_2, O_3, O_4O2​,O3​,O4​는 큰 객체 O1O_1O1​이 끝나야 전송 시작 가능
    - 즉, **Head-of-Line Blocking (HOL Blocking)** 발생

### 요약

- **HTTP/1.1**: FCFS 처리 → 작은 객체도 큰 객체 뒤에서 대기   
- **문제점**: 응답 지연 증가, 사용자 경험 저하


## HTTP/2: Mitigating HOL Blocking

### 방법

- HTTP/1.1과 달리, **객체를 프레임 단위로 분할**
- 여러 객체의 프레임을 **교차(interleaved)** 하여 전송
- 큰 객체 O1O_1O1​도 전송되지만, 작은 객체들 O2,O3,O4O_2, O_3, O_4O2​,O3​,O4​의 프레임을 중간중간 섞어서 전송

### 결과

- 작은 객체 O2,O3,O4O_2, O_3, O_4O2​,O3​,O4​: **빠르게 전달**    
- 큰 객체 O1O_1O1​: 약간 늦어지지만 전체 지연 감소
- **HOL Blocking 완화** → 사용자 체감 속도 개선

📌 요약

- **HTTP/1.1**: 큰 객체가 먼저 전송되면 작은 객체는 대기해야 함 → HOL Blocking 심각    
- **HTTP/2**: 객체를 프레임 단위로 쪼개어 동시 전송 → HOL Blocking 완화, 작은 객체 빠르게 수신 가능


## HTTP/2 → HTTP/3

### Key Goal

- 여러 객체 요청 시 지연 감소

### HTTP/2의 한계 (TCP 기반)

- **단일 TCP 연결** 위에서 동작    
- **패킷 손실 발생 시** → 모든 객체 전송이 지연됨 (stalling)
    - → 브라우저는 이를 피하려고 **여러 개의 병렬 TCP 연결**을 열어 사용
- 기본 TCP 연결은 **보안(Encryption)**이 없음

### HTTP/3 (UDP 기반, QUIC 프로토콜)

- **보안 강화**: 암호화 기본 적용    
- **객체 단위 오류 및 혼잡 제어** 가능
- UDP 기반 멀티플렉싱 → **패킷 손실 시 다른 객체 전송은 영향 없음**
- 더 많은 파이프라이닝 지원

📌 요약

- **HTTP/2**: HOL 완화는 가능했지만, TCP 특성 때문에 여전히 패킷 손실에 취약    
- **HTTP/3**: QUIC(UDP 기반)을 사용하여 보안 + 성능 + 신뢰성을 동시에 제공


## E-mail

### Three Major Components

1. **User Agents (UA)**
    - 메일 작성, 편집, 읽기 담당
    - 예: Outlook, iPhone mail client
    - 별칭: “mail reader”

2. **Mail Servers**    
    - 사용자 메일박스(user mailbox)와 발신 메시지 큐(outgoing message queue) 저장
    - 메일 송수신의 중계 역할

3. **SMTP (Simple Mail Transfer Protocol)**    
    - 메일 서버 간 메시지 전송을 위한 프로토콜
    - 발신 서버에서 수신 서버로 메시지 전달

### User Agent (UA)

- 사용자가 메일을 **작성(composing)**, **편집(editing)**, **읽기(reading)** 가능    
- 송신(outgoing), 수신(incoming) 메시지는 서버에 저장됨
- 예시: Outlook, iPhone mail client

📌 요약

- 이메일 시스템은 **User Agent + Mail Server + SMTP**로 구성    
- User Agent는 **메일 클라이언트** 역할, Mail Server는 **저장·전송 중계**, SMTP는 **프로토콜**


## E-mail: Mail Servers

### Mail Server 기능

1. **Mailbox**
    - 사용자별로 존재
    - 수신된 메시지(incoming messages) 저장

2. **Message Queue**    
    - 발신 메시지(outgoing messages) 저장
    - 아직 전송되지 않은 메일들을 보관

### SMTP Protocol

- **메일 서버 간** 메시지 전달에 사용    
- 송신 메일 서버 = **client** 역할
- 수신 메일 서버 = **server** 역할

📌 요약

- **Mailbox**: 사용자 수신 메일 보관    
- **Message Queue**: 발신 대기 메일 저장
- **SMTP**: 메일 서버 간 전송을 담당 (클라이언트–서버 관계 형성)


## E-mail: RFC 5321 (SMTP)

### 전송 방식

- **TCP 기반**으로 신뢰성 있게 메시지 전송
- 포트 번호: **25번**
- 송신 서버(클라이언트 역할) → 수신 서버(서버 역할) **직접 전송**

### 전송 과정 (3단계)

1. **Handshaking (인사/연결)**    
    - 연결 성립 및 인사 교환

2. **Transfer of messages (메시지 전송)**    
    - 실제 메일 데이터 전송

3. **Closure (종료)**    
    - 세션 종료

### Command/Response 구조

- **Command**: 클라이언트가 ASCII 텍스트 명령 전송    
- **Response**: 서버가 상태 코드 + 메시지로 응답
- (HTTP와 유사한 상호작용 구조)

### 메시지 형식

- 반드시 **7-bit ASCII**로 인코딩    

📌 요약

- **SMTP (RFC 5321)**는 TCP 25번 포트 사용    
- 전송 과정: Handshake → Message Transfer → Close
- 명령/응답 구조, 메시지는 7-bit ASCII 기반


## Scenario: Alice sends e-mail to Bob

1. **Alice 작성**
    - Alice는 User Agent(UA)를 사용해 이메일 작성
    - 수신자: `bob@someschool.edu`

2. **Alice → Mail Server**    
    - Alice의 UA가 메시지를 Alice의 메일 서버로 전송
    - 메시지는 **메시지 큐(message queue)**에 저장

3. **SMTP 연결 생성**    
    - Alice의 메일 서버가 SMTP 클라이언트 역할 수행
    - TCP 연결을 Bob의 메일 서버와 설정

4. **메시지 전송**    
    - SMTP 클라이언트가 TCP 연결을 통해 메시지 전송

5. **Bob의 Mailbox 저장**    
    - Bob의 메일 서버가 메시지를 수신
    - 메시지를 Bob의 **메일박스(mailbox)**에 저장

6. **Bob 읽기**    
    - Bob은 자신의 User Agent를 실행하여 메시지를 확인

📌 요약

- 이메일은 **UA → 발신자 메일 서버 → 수신자 메일 서버 → UA** 흐름으로 전송됨    
- SMTP는 서버 간 메시지 전달을 담당
- 메일박스(mailbox)와 메시지 큐(message queue)가 중간 저장소 역할


## Sample SMTP Interaction

### 통신 흐름

- **S**: Server (수신 메일 서버)
- **C**: Client (송신 메일 서버)

### 단계별 과정

1. **연결 및 인사**    
    - `S: 220 hamburger.edu` → 서버 연결 준비 완료
    - `C: HELO crepes.fr` → 클라이언트 인사
    - `S: 250 Hello crepes.fr, pleased to meet you` → 서버 응답

2. **발신자 지정**    
    - `C: MAIL FROM: <alice@crepes.fr>`
    - `S: 250 alice@crepes.fr... Sender ok`

3. **수신자 지정**    
    - `C: RCPT TO: <bob@hamburger.edu>`
    - `S: 250 bob@hamburger.edu... Recipient ok`

4. **메시지 데이터 전송**    
    - `C: DATA`
    - `S: 354 Enter mail, end with "." on a line by itself`
    - `C: Do you like ketchup?`
    - `C: How about pickles?`
    - `C: .` (마침표로 메시지 종료 표시)
    - `S: 250 Message accepted for delivery`

5. **세션 종료**    
    - `C: QUIT`
    - `S: 221 hamburger.edu closing connection`

📌 요약

- SMTP는 **명령/응답(command/response)** 기반    
- 주요 명령어: `HELO`, `MAIL FROM`, `RCPT TO`, `DATA`, `QUIT`
- 메시지 본문은 `DATA` 이후 전송, `.` 단독 줄로 종료


## SMTP: Closing Observations

### HTTP와 비교

- **HTTP**: **Pull 방식** (클라이언트가 요청 → 서버 응답)
- **SMTP**: **Push 방식** (송신 서버가 직접 수신 서버로 전송)
- 공통점: **ASCII 기반 명령/응답 구조**, 상태 코드 사용
- **HTTP**: 각 객체가 **독립적인 응답 메시지**로 캡슐화
- **SMTP**: 여러 객체를 **multipart message**로 묶어 전송

### SMTP 추가 특징

- **Persistent connections** (지속 연결 사용)    
- 메시지(헤더 + 본문)는 반드시 **7-bit ASCII** 형식
- 메시지 종료 구분: `CRLF.CRLF` 사용

📌 요약

- SMTP와 HTTP 모두 명령/응답 구조를 사용하지만,    
    - HTTP는 **클라이언트 pull 기반**
    - SMTP는 **서버 간 push 기반**

- SMTP는 지속 연결을 활용하며, 메시지 종료를 CRLF.CRLF로 표시


## Mail Message Format

### 정의

- **SMTP (RFC 5321)**: 이메일 메시지 전송을 위한 프로토콜 (HTTP와 유사)
- **RFC 5322**: 이메일 메시지 자체의 **구문(syntax)** 정의 (HTML과 유사한 역할)

### 구성 요소

1. **Header (헤더)**    
    - 예:
        - `From:` 송신자 주소   
        - `To:` 수신자 주소
        - `Subject:` 메일 제목
    - 주의: 이 헤더는 **SMTP 명령어(MAIL FROM, RCPT TO)**와 다름

2. **Body (본문)**    
    - 실제 메시지 내용
    - **ASCII 문자만 허용 (7-bit)**

### 구분 방법

- **Header와 Body 사이**: 반드시 **빈 줄(blank line)**로 구분    

📌 요약

- 이메일 메시지는 **Header + Body**로 구성    
- Header: 메타데이터 (발신자, 수신자, 제목)
- Body: 실제 메시지, ASCII만 허용
- 구분: **빈 줄(blank line)**


## Mail Access Protocols

### 1. SMTP (Simple Mail Transfer Protocol)

- 역할: **송신자 메일 서버 → 수신자 메일 서버**로 이메일 전달/저장
- 단점: **사용자가 서버에서 메일을 읽는 기능은 제공하지 않음**

### 2. Mail Access Protocols (사용자가 메일 서버에서 메일을 가져올 때 사용)

- **POP3 (Post Office Protocol v3, RFC 1939)**    
    - 단순한 프로토콜, 기능 제한적
    - 메일을 클라이언트로 가져오고 서버에서는 삭제하는 방식 (기본 동작)

- **IMAP (Internet Mail Access Protocol, RFC 3501)**    
    - 메일이 서버에 저장됨
    - 클라이언트는 서버에 접근하여 **조회, 삭제, 폴더 관리** 가능
    - 여러 디바이스에서 동기화 가능

### 3. HTTP (웹 메일)

- Gmail, Hotmail, Yahoo! Mail 등    
- **웹 기반 인터페이스 제공**
- 내부적으로:
    - SMTP (메일 송신)
    - IMAP/POP3 (메일 수신)

📌 요약

- **SMTP**: 서버 간 메일 전달    
- **POP3**: 단순 다운로드 방식 (서버에 메일 유지 제한적)
- **IMAP**: 서버 중심, 다양한 기능 제공 (멀티 디바이스에 적합)
- **HTTP**: 웹 메일 서비스, SMTP + IMAP/POP 기반


## 풀이

### 1. Incoming Mail Server (수신 서버)

- Outlook 설정에서 **Account Type = POP3**로 지정됨
- 따라서 **POP 서버 주소** 필요 → `pop.gmail.com`

### 2. Outgoing Mail Server (발신 서버)

- 발신은 항상 **SMTP** 사용   
- 따라서 주소는 → `smtp.gmail.com`

## 정답

- **Incoming mail server**: `pop.gmail.com` (선택지 ③)    
- **Outgoing mail server**: `smtp.gmail.com` (선택지 ⑤)

📌 참고:

- IMAP을 사용할 경우: `imap.gmail.com`    
- POP3를 사용할 경우: `pop.gmail.com`
- 발신(SMTP)은 항상: `smtp.gmail.com`


## DNS: Domain Name System

### 1. 사람과 인터넷 비교

- **사람(people)** → 여러 가지 식별자 사용
    - 주민번호, 이름, 여권번호 등

- **인터넷 호스트/라우터** → 두 가지 식별자 사용    
    - **IP 주소 (32비트)** → 데이터 전송용
    - **도메인 이름 (예: cs.umass.edu)** → 사람이 쓰기 쉬움

👉 문제: IP 주소와 이름을 어떻게 연결할까?

### 2. DNS란?

- **분산 데이터베이스**   
    - 여러 네임 서버가 계층적으로 연결됨

- **애플리케이션 계층 프로토콜**    
    - 호스트 ↔ 네임 서버가 통신하여  
        도메인 이름을 IP 주소로 변환(resolve)

### 3. 특징

- 인터넷 핵심 기능이지만,  
    **애플리케이션 계층에서 동작**    
- 네트워크 중심부가 아닌 **엣지(Edge)** 에 복잡성이 있음

👉 **정리:**  
DNS는 사람이 쓰는 **도메인 이름**과 컴퓨터가 쓰는 **IP 주소**를 서로 연결해 주는 시스템이다.


## DNS: 동작 방식 (How it works)

사용자가 **www.handong.edu** 에 접속하려고 할 때:

1. 사용자 컴퓨터에서 **DNS 클라이언트 프로그램**이 실행됨.
2. 브라우저는 URL에서 **호스트 이름(www.handong.edu)** 을 추출하여 DNS 클라이언트에 전달함.
3. DNS 클라이언트는 **호스트 이름을 포함한 질의(Query)** 를 DNS 서버로 전송함.
4. DNS 서버는 **호스트 이름에 해당하는 IP 주소** 를 찾아서 DNS 클라이언트에 응답함.
5. 브라우저는 받은 **IP 주소** 를 사용하여, 해당 서버(IP)의 80번 포트(HTTP 서버)로 **TCP 연결**을 시작함.

👉 **정리:**  
DNS는 브라우저가 입력한 **도메인 이름 → IP 주소** 변환을 해주고,  
브라우저는 그 IP를 이용해 실제 웹 서버와 연결한다.


## DNS: 서비스와 구조

### DNS 제공 서비스

- **호스트 이름 → IP 주소 변환**
- **호스트 별칭 (aliasing)**
    - 정식 이름(canonical), 별칭(alias) 이름 지원

- **메일 서버 별칭**    
- **부하 분산 (load distribution)**
    - 복제된 웹 서버 운영: 하나의 도메인 이름이 여러 IP 주소와 매핑됨

### 중앙 집중식 DNS는 왜 안 될까?

- **단일 장애 지점 (single point of failure)**    
- **트래픽 과부하**
- **중앙 DB와의 거리 문제**
- **유지보수 어려움**

### 답: 확장성 문제 (doesn’t scale)

- 예: Comcast DNS 서버만 해도 하루 **6000억 건 이상의 DNS 질의** 처리    

👉 **정리:**  
DNS는 전 세계적으로 분산 구조로 운영되어야 하며,  
중앙 집중 방식으로는 트래픽과 장애에 대응할 수 없다.


## DNS: 분산된 계층형 데이터베이스

### 계층 구조

- **Root DNS 서버**
    - 최상위 계층, TLD 서버를 가리킴

- **Top Level Domain (TLD) DNS 서버**    
    - 예: `.com`, `.org`, `.edu`

- **권한 있는(Authoritative) DNS 서버**    
    - 실제 도메인(예: amazon.com, yahoo.com)의 IP 주소를 알려줌


### 예시: `www.amazon.com` IP 주소 찾기

1. 클라이언트 → **Root 서버**에 질의    
    - `.com DNS 서버` 위치를 알려줌

2. 클라이언트 → **.com DNS 서버**에 질의    
    - `amazon.com DNS 서버` 위치를 알려줌

3. 클라이언트 → **amazon.com DNS 서버**에 질의    
    - `www.amazon.com` 의 실제 **IP 주소**를 알려줌


👉 **정리:**  
DNS는 **Root → TLD → Authoritative** 순서로 내려가면서,  
	도메인 이름에 해당하는 IP 주소를 찾아낸다.


## DNS: 루트 네임 서버 (Root Name Servers)

### 역할

- 다른 네임 서버들이 이름을 해결하지 못할 때 **마지막으로 참조하는 공식 서버**
- 인터넷에서 **매우 중요한 핵심 기능**
    - 루트 서버 없이는 인터넷이 동작 불가
    - **DNSSEC 지원** → 보안 제공 (인증, 무결성 보장)

### 관리

- **ICANN** (Internet Corporation for Assigned Names and Numbers)    
    - 루트 DNS 도메인을 관리

### 전 세계 배치

- **논리적 루트 서버 13개** 존재    
- 각 서버는 여러 번 복제되어 전 세계에 분산 배치됨
    - 미국에만 약 **200개 이상** 존재


👉 **정리:**  
루트 네임 서버는 DNS 계층 구조의 **최상위에 있는 서버**로,  
전 세계 인터넷 동작에 반드시 필요한 핵심 인프라이다.


## TLD: 권한 있는 서버 (Authoritative Servers)

### Top-Level Domain (TLD) 서버

- `.com`, `.org`, `.net`, `.edu`, `.aero`, `.jobs`, `.museums` 와 같은 **최상위 도메인** 관리    
- 국가 도메인도 포함 (예: `.cn`, `.uk`, `.fr`, `.ca`, `.jp`)
- 예시
    - **Network Solutions** → `.com`, `.net` TLD 관리
    - **Educause** → `.edu` TLD 관리

### 권한 있는 DNS 서버 (Authoritative DNS Servers)

- 조직(organization)의 자체 DNS 서버    
    - 도메인 이름 ↔ IP 주소 매핑 정보 보관

- 조직 또는 서비스 제공자(service provider)가 관리 가능    


👉 **정리:**  
TLD 서버는 **최상위 도메인**을 관리하고,  
**권한 있는 DNS 서버**는 실제 도메인에 대한 IP 주소를 최종적으로 알려준다.


## Local DNS Name Servers

### 특징

- **DNS 계층 구조(hierarchy)에 엄밀히 속하지 않음**
- 각 ISP(통신사, 회사, 대학 등)는 자체 **로컬 DNS 서버**를 가짐
    - "기본 네임 서버(default name server)" 라고도 부름


### 동작 방식

- 호스트가 DNS 질의를 하면, 먼저 **로컬 DNS 서버**로 전송됨    
- 로컬 DNS 서버는
    - **최근 질의 결과 캐시(local cache)** 를 보관 (단, 오래되었을 수 있음)
    - **프록시 역할**을 하여, 계층 구조 상위 DNS 서버로 질의를 전달


👉 **정리:**  
로컬 DNS 서버는 사용자가 가장 먼저 접근하는 DNS 서버로,  
캐시를 활용해 빠르게 응답하거나, 계층 구조에 질의를 대신 전달한다.


## DNS 이름 해석: 반복 질의 (Iterated Query)

### 예시

- **요청 호스트**: `engineering.nyu.edu`
- **찾고 싶은 주소**: `gaia.cs.umass.edu` 의 IP

### 반복 질의(Iterated Query) 방식

- 질의받은 서버가 직접 답을 주지 않고,  
    **“나는 모르지만, 이 서버에 물어봐라”** 라고 다음 서버를 알려줌    
- 클라이언트(로컬 DNS)가 순서대로 여러 서버를 거쳐 최종 답을 얻음


### 동작 흐름

1. `engineering.nyu.edu` 호스트 → 로컬 DNS (`dns.nyu.edu`)    
2. 로컬 DNS → 루트 DNS 서버
3. 루트 DNS → “.edu TLD 서버에 물어보라” 응답
4. 로컬 DNS → .edu TLD DNS 서버
5. TLD 서버 → “umass.edu 권한 서버에 물어보라” 응답
6. 로컬 DNS → `umass.edu` 권한 DNS 서버
7. 권한 DNS 서버 → `gaia.cs.umass.edu` 의 IP 주소 전달
8. 로컬 DNS → 원래 호스트에 최종 IP 주소 전달

👉 **정리:**  
반복 질의는 클라이언트가 여러 DNS 서버를 직접 순차적으로 거쳐서,  
최종적으로 **도메인 이름에 해당하는 IP 주소**를 얻는 방식이다.

## Recursive Query (재귀 질의)

### 특징

- **이름 해석(name resolution)의 부담**을 질의받은 DNS 서버가 짐
- 상위 계층 서버(root, TLD)에 **부하가 많이 걸릴 수 있음**

👉 **정리:**  
재귀 질의는 클라이언트가 아닌 **서버가 대신 다음 서버에 계속 질의**하여 최종 IP 주소를 찾아 클라이언트에게 전달하는 방식이다.



## DNS Exercise

### 조건

- 클라이언트 ↔ 로컬 DNS 서버 RTT = **RTTₗ**
- 로컬 DNS 서버 ↔ 다른 DNS 서버 RTT = **RTTᵣ**
- DNS 서버는 **캐싱 없음** (단, c에서 캐싱 고려)

### 문제

**a. 시나리오 A의 총 응답 시간은?**

- 클라이언트 ↔ 로컬 DNS: **RTTₗ**    
- 로컬 DNS ↔ Root DNS: **RTTᵣ**
- 로컬 DNS ↔ TLD DNS: **RTTᵣ**
- 로컬 DNS ↔ Authoritative DNS: **RTTᵣ**

👉 총 응답 시간 = **RTTₗ + 3RTTᵣ**

**b. 시나리오 B의 총 응답 시간은?**

- 클라이언트 ↔ 로컬 DNS: **RTTₗ**    
- 로컬 DNS ↔ Root DNS: **RTTᵣ**
- Root ↔ TLD DNS: **RTTᵣ**
- TLD ↔ Authoritative DNS: **RTTᵣ**

👉 총 응답 시간 = **RTTₗ + 3RTTᵣ**  
(시나리오 A와 동일)

**c. 로컬 DNS 서버가 요청한 이름의 캐시를 가지고 있다면?**

- 클라이언트 ↔ 로컬 DNS: **RTTₗ**    
- 추가 질의 불필요

👉 총 응답 시간 = **RTTₗ** (양쪽 시나리오 동일)

👉 **정리:**

- 시나리오 A = RTTₗ + 3RTTᵣ    
- 시나리오 B = RTTₗ + 3RTTᵣ
- 캐싱 있는 경우 = RTTₗ


## DNS 레코드 캐싱과 갱신

### 캐싱 동작

- 어떤 네임 서버든 매핑(IP ↔ 이름)을 알게 되면 **캐시에 저장**
- 캐시 항목은 일정 시간(TTL)이 지나면 삭제됨
- 보통 **로컬 네임 서버가 TLD 서버 결과를 캐싱** → 루트 서버는 자주 호출되지 않음

### 문제점

- 캐시된 정보는 **구식(out-of-date)** 일 수 있음    
    - → “최선의 노력(best-effort)” 기반의 이름-주소 변환

- 만약 호스트가 **IP 주소를 변경**해도, TTL이 만료되기 전까지는 인터넷 전역에 반영되지 않음    

### 표준화

- **갱신/알림(update/notify) 메커니즘**    
- IETF 표준 제안: **RFC 2136**

👉 **정리:**  
DNS 캐싱은 성능 향상에 유리하지만, **IP 변경 시 전파 지연 문제**가 발생할 수 있다.


## DNS Records (자원 레코드, RR)

### RR 형식

- **(name, value, type, ttl)**

### 주요 타입

- **A 레코드 (type=A)**   
    - name = 호스트 이름
    - value = IP 주소

- **NS 레코드 (type=NS)**    
    - name = 도메인 이름 (예: foo.com)
    - value = 해당 도메인의 권한 있는 네임 서버(hostname)

- **CNAME 레코드 (type=CNAME)**    
    - name = 별칭(alias) 이름
    - value = 실제 정식(canonical) 이름
    - 예: `www.ibm.com` → `servereast.backup2.ibm.com`

- **MX 레코드 (type=MX)**    
    - value = 해당 도메인의 메일 서버 이름


👉 **정리:**  
DNS 레코드는 **도메인 이름 ↔ IP 주소, 네임 서버, 별칭, 메일 서버**와 같은 매핑 정보를 저장하는 구조이다.


## DNS Protocol Messages

### 형식 (Query와 Reply 동일)

- **메시지 헤더 구조**
    - **Identification**
        - 16비트 번호 (Query ↔ Reply 매칭용)

    - **Flags**        
        - Query인지 Reply인지
        - Recursion 원하는지 표시
        - Recursion 가능한지 표시
        - Reply가 권한(authoritative) 응답인지 여부

### 메시지 구성

1. **Identification (2 bytes)**    
2. **Flags (2 bytes)**
3. **# Questions** (질문 개수)
4. **# Answer RRs** (응답 자원 레코드 개수)
5. **# Authority RRs** (권한 서버 정보 개수)
6. **# Additional RRs** (추가 정보 개수)
7. **Questions** (가변 길이)
8. **Answers** (가변 길이)
9. **Authority** (가변 길이)
10. **Additional Info** (가변 길이)

👉 **정리:**  
DNS 메시지는 **Query/Reply 모두 같은 구조**를 가지며,  
**Identification + Flags + RRs(질문/응답/권한/추가)** 로 이루어진다.


## DNS Protocol Messages (Query & Reply)

### 공통 형식

- **Identification (16bit)**    
    - Query ↔ Reply 매칭용 번호

- **Flags**    
    - Query/Reply 여부, 재귀 요청 여부, 권한 응답 여부 등

- **# Questions / # Answer RRs / # Authority RRs / # Additional RRs**
    - 각각 질문, 응답, 권한 서버 정보, 추가 정보 개수        

### 주요 필드

- **Questions**: 질의 내용 (이름, 타입)
- **Answers**: 질의에 대한 응답 RR(Resource Record)
- **Authority**: 권한 있는 서버 정보
- **Additional Info**: 추가적인 “도움이 될 수 있는” 정보

👉 **정리:**  
DNS Query와 Reply 메시지는 동일한 구조를 가지며,  
**헤더 + 질문 + 응답 + 권한 서버 + 추가 정보** 로 구성된다.

키워드만 외워도 절반 + 나머지는 이것들이 어떻게 기능하는가


## Inserting records into DNS (DNS에 레코드 삽입하기)

### 예시: 새로운 스타트업 “Network Utopia”

1. **도메인 등록**
    - `networkuptopia.com`을 **DNS 등록 기관(DNS registrar)** (예: Network Solutions)에 등록합니다.

    - 등록 시 필요한 사항:        
        - 권한 있는 이름 서버(authoritative name server)의 이름과 IP 주소 제공 (주/보조)
        - 등록 기관은 `.com` TLD 서버에 **NS, A 레코드**를 삽입합니다.
            `(networkuptopia.com, dns1.networkuptopia.com, NS) (dns1.networkuptopia.com, 212.212.212.1, A)`

2. **권한 있는 서버(authoritative server) 생성**    
    - IP 주소 `212.212.212.1`을 사용하여 로컬에서 서버를 구축합니다.
    - 서버에 등록할 레코드 예시:
        - A 레코드: `www.networkuptopia.com`
        - MX 레코드: `networkuptopia.com`

## 추가 설명

- **DNS registrar**: 사용자가 도메인을 구매하고, 그 도메인을 TLD 서버(.com, .org 등)에 연결할 수 있도록 해 주는 기관입니다.    
- **NS 레코드(Name Server record)**: 해당 도메인의 권한 있는 DNS 서버의 주소를 알려주는 레코드입니다.
- **A 레코드(Address record)**: 특정 도메인 이름을 실제 IP 주소로 매핑합니다.
- **MX 레코드(Mail Exchange record)**: 메일 서버 정보를 담고 있으며, 이메일이 해당 도메인으로 전달될 수 있도록 합니다.
- **권한 있는 서버(authoritative server)**: 해당 도메인 이름에 대한 최종적인 응답을 제공하는 DNS 서버입니다.

즉, 새로운 도메인을 만들려면 **① 도메인 등록기관에 등록 → ② TLD 서버에 NS와 A 기록 삽입 → ③ 자체 DNS 서버를 구축하여 A/MX 등의 서비스용 레코드 등록** 과정을 거칩니다.


## Peer-to-peer (P2P) architecture

### 특징

- **항상 켜져 있는 서버가 없음 (no always-on server)**
- **임의의 종단 시스템(end systems)이 직접 통신**
- **피어(peer)는 다른 피어에게 서비스를 요청하고, 동시에 서비스 제공**
    - _자체 확장성(self scalability)_: 새로운 피어가 들어오면 새로운 서비스 용량을 제공하면서 동시에 새로운 서비스 수요도 발생시킴

- **피어는 간헐적으로 접속하고 IP 주소가 바뀔 수 있음**    
    - 관리가 복잡해짐

### 예시

- **P2P 파일 공유**: BitTorrent
- **스트리밍**: KanKan
- **VoIP**: Skype
- **블록체인**


## 추가 설명

- **P2P 구조**는 중앙 서버 없이 개별 사용자가 직접 서로 연결되어 자원을 공유하는 방식입니다   
- 새로운 사용자가 참여하면 시스템의 전체 자원(저장 공간, 대역폭)이 늘어나면서도 동시에 새로운 부하가 발생하는 **자기 확장성(self scalability)**의 특징을 가집니다.
- 하지만 사용자가 항상 접속해 있는 것이 아니고, 접속할 때마다 IP 주소가 변할 수 있어 **네트워크 관리가 복잡**하다는 단점이 있습니다.
- 대표적으로 **토렌트(BT)** 같은 파일 공유, **스카이프(Skype)** 같은 인터넷 전화, 그리고 **블록체인 네트워크**가 P2P 방식으로 동작합니다.


## File distribution: client-server vs P2P

### 질문 (Q)

- **한 서버에서 N개의 피어(peer)로 크기 F인 파일을 분배하는 데 얼마나 시간이 걸리는가?**

### 조건
- 피어의 **업로드/다운로드 용량**은 제한된 자원임.

### 변수

- **서버 업로드 용량**: `u_s`    
- **피어 i의 다운로드 용량**: `d_i`
- **피어 i의 업로드 용량**: `u_i`
- **파일 크기**: `F`

### 그림 설명

- 서버가 파일(크기 F)을 네트워크를 통해 피어들에게 전송.
- 각 피어는 **다운로드(d_i)**와 **업로드(u_i)** 능력을 가지고 있음.
- 네트워크 자체의 대역폭은 풍부하다고 가정. (즉, 병목은 피어와 서버의 업/다운 속도에서 발생)

## 추가 설명

- **클라이언트-서버 방식**에서는 모든 파일을 서버가 직접 업로드해야 하므로 서버 업로드 속도(`u_s`)가 병목이 됨. 따라서 피어 수(N)가 많아질수록 서버의 부담이 커짐.
- **P2P 방식**에서는 피어도 파일을 다운로드하면서 동시에 다른 피어에게 업로드할 수 있음. 즉, 전체 업로드 용량이 `u_s + Σ u_i` 로 확장되어 **확장성**이 뛰어남.
- 이 차이가 **대규모 분산 환경에서 P2P가 선호되는 이유** 중 하나임. (예: 토렌트, 블록체인)


## File distribution time: client-server

### 서버 전송 (server transmission)

- 서버는 **N개의 클라이언트에 대해 각각 파일 복사본을 순차적으로 업로드**해야 함.
    - 한 개 파일을 보내는 시간: `F / u_s`
    - N개 파일을 보내는 시간: `N·F / u_s`

### 클라이언트 (client)

- 각 클라이언트는 자신의 파일 복사본을 다운로드해야 함.
- `d_min` = **최소 다운로드 속도(min client download rate)**
- 최소 다운로드 시간: `F / d_min`

### 전체 분배 시간 (distribution time)

D_C−S≥max⁡{NF/u_s,F/d_min}

- 즉, 전체 분배 시간은 **서버 업로드 용량**과 **최저 다운로드 속도** 중 더 오래 걸리는 쪽에 의해 결정됨.
- 결과적으로, **클라이언트 수(N)가 증가할수록 분배 시간은 선형적으로 증가**함.

## 추가 설명

- **병목 지점 1: 서버 업로드 용량**  
    서버는 혼자서 N명의 클라이언트에 파일을 모두 전송해야 하므로, 업로드 속도가 분배 시간의 핵심 제약이 됨.    
- **병목 지점 2: 느린 클라이언트**  
    다운로드 속도가 가장 느린 클라이언트(`d_min`)는 전체 분배 과정의 최소 시간을 결정하는 또 다른 제약 요소임.
- 따라서 클라이언트-서버 구조에서는 **N이 커질수록 확장성 문제가 발생**하며, 이는 P2P 구조가 등장한 중요한 배경 중 하나임.


## File distribution time: P2P

### 서버 전송 (server transmission)

- 서버는 최소한 한 개의 파일 복사본은 업로드해야 함.
- 한 개 파일 업로드 시간: `F / u_s`

### 클라이언트 (client)

- 각 클라이언트는 자신의 파일 복사본을 다운로드해야 함.
- 최대 다운로드 시간: `F / d_min`

### 전체 클라이언트 (clients as a group)

- 모든 클라이언트가 합쳐서 `N·F` 비트를 다운로드해야 함.
- 최대 업로드 속도(= 전체 다운로드를 제한하는 속도): `u_s + Σ u_i`

### 전체 분배 시간 (distribution time)

D_P2P≥max⁡{F/u_s, F/d_min, NF/u_s+∑u_i}

- 즉, 전체 분배 시간은 서버 업로드, 가장 느린 클라이언트의 다운로드 속도, 그리고 전체 업로드 용량에 의해 결정됨.
- 클라이언트 수 N이 증가하면 분배 시간도 선형적으로 증가하지만, 동시에 각 피어가 새로운 업로드 용량을 제공하므로 **확장성이 우수**함.

## 추가 설명

- **클라이언트-서버 구조와 차이점**    
    - 클라이언트-서버에서는 서버 업로드 용량이 병목이 됨.
    - P2P에서는 새로운 클라이언트가 참여할수록 업로드 자원이 늘어나므로, N이 커져도 효율적으로 확장 가능.

- **핵심 개념**    
    - P2P 구조는 **자체 확장성(self scalability)**을 제공함.
    - 참여자가 많아질수록 자원도 함께 증가하여, 대규모 분산 환경에 적합함. (예: 토렌트, 블록체인)


![[Pasted image 20250929102327.png]]



## Exercise

### 문제 설명

- **파일 크기**: F=15 Gbits    
- **서버 업로드 속도**: us=30 Mbps
- **피어 다운로드 속도**: di=2 Mbps
- **피어 업로드 속도**: u=300 Kbps,700 Kbps,2 Mbps    
- **피어 수 (N)**: N=10,100,1000

**과제:**  
각 (N,u) 조합에 대해,

- 클라이언트-서버 구조
- P2P 구조  
    에서의 최소 분배 시간(minimum distribution time)을 계산하여 표(chart)로 정리하라.

## 공식 (힌트 제공)

- **클라이언트-서버 구조**    

D_C−S=max⁡{NF/u_s, F/d_min⁡}

- **P2P 구조**

D_P2P=max⁡{F/u_s, F/d_min⁡, NF/u_s+∑u_i}

여기서 ∑u_i=N⋅u

## 추가 설명

- **클라이언트-서버**: 서버 혼자 모든 피어에게 파일을 나눠주므로, 분배 시간은 **서버 업로드 용량**과 **가장 느린 클라이언트 다운로드 속도**에 의해 결정됩니다.
- **P2P**: 각 피어도 업로드에 기여하므로, 전체 업로드 용량이 us+N⋅uu_s + N \cdot uus​+N⋅u로 확장됩니다. 따라서 피어 수가 늘어나도 훨씬 효율적입니다.


## P2P file distribution: BitTorrent

### 기본 개념

- 파일은 **256Kb 크기의 청크(chunks)** 단위로 분할됨.
- 토렌트에 참여하는 피어들은 파일 청크를 서로 **업로드/다운로드**하며 교환함.

### 주요 구성 요소

- **Tracker**: 토렌트에 참여하는 피어들을 추적(tracking)하는 서버.
- **Torrent**: 파일 청크를 서로 교환하는 피어들의 그룹.

### 동작 예시

- 새로운 사용자가 접속(Alice arrives) → 트래커로부터 현재 참여 중인 피어들의 목록을 받음.
- Alice는 그 목록에 있는 피어들과 파일 청크를 교환하기 시작함.

## 추가 설명

- **BitTorrent 방식의 특징**    
    - 파일을 작은 단위로 쪼개 교환하므로 다운로드 속도가 빠름.
    - 사용자는 다운로드와 동시에 업로드에도 기여해야 함.
    - 중앙 서버는 파일 데이터를 직접 전송하지 않고, **트래커(Tracker)** 역할만 수행 → 네트워크 부하 분산 가능.

- **장점**    
    - 많은 사용자가 참여할수록 전체 전송 속도가 향상됨 (**자체 확장성**).
    - 서버의 업로드 부담이 최소화됨.

- **구조 요약**    
    - Tracker: “누가 참여하고 있는지 알려주는 안내자”
    - Torrent: “실제로 파일 조각을 교환하는 네트워크 집단”



## P2P file distribution: BitTorrent

### 피어가 토렌트에 참여할 때 (peer joining torrent)

- 처음에는 **파일 청크가 없음** → 시간이 지나면서 다른 피어들로부터 청크를 받아 모음.    
- 트래커(tracker)에 등록하여 피어 목록을 받고, 일부 피어(“neighbors”)와 연결함.

### 다운로드 중 행동

- 다운로드를 진행하면서 동시에 **다른 피어에게 청크를 업로드**함.
- 교환하는 피어를 **변경**할 수도 있음.

### 네트워크 특성

- **Churn**: 피어들이 네트워크에 **왔다 갔다 함(접속/이탈)**.
- 한 피어가 전체 파일을 모두 받은 후에는:
    - **이기적으로(selfishly)** → 네트워크를 떠남.
    - **이타적으로(altruistically)** → 계속 남아서 다른 피어에게 업로드를 제공.

## 추가 설명

- **BitTorrent의 핵심 원리**:    
    - 피어는 단순히 받기만 하는 것이 아니라, 동시에 다른 사람에게 주어야 함.
    - 네트워크 전체가 **서로의 업로드 기여로 유지**됨.
- **Churn 현상**은 P2P 네트워크 관리의 어려움 중 하나임. 피어가 언제든 떠날 수 있기 때문에, 토렌트는 **여러 개의 청크를 중복 배포**하여 안정성을 확보함.
- 따라서 BitTorrent는 단순히 파일을 빠르게 전송하는 시스템이 아니라, **참여자들의 협력적 행위에 기반한 분산 네트워크**라 할 수 있음.


## BitTorrent: requesting, sending file chunks

### Requesting chunks (청크 요청)

- 특정 시점에서, 피어마다 서로 다른 **파일 청크 집합**을 가지고 있음.    
- 주기적으로, Alice는 각 피어에게 그들이 가진 청크 목록을 요청함.
- Alice는 자신에게 없는 청크를 요청하되, **가장 희귀한(rarest) 청크부터 먼저 요청**함.

### Sending chunks (청크 전송) — _tit-for-tat_ 전략

- Alice는 현재 자신에게 **가장 높은 속도로 청크를 보내주고 있는 4명의 피어**에게 청크를 전송함.
    - 다른 피어들은 **choked** 상태가 되어, Alice로부터 청크를 받지 못함.
    - 이 상위 4명은 10초마다 다시 평가(re-evaluate)됨.

- 매 30초마다: 무작위로 다른 피어를 선택하여 청크 전송을 시작함.    
    - 이 피어는 **optimistically unchoked** 상태가 됨.
    - 새로 선택된 피어가 상위 4명 안에 들어갈 수도 있음.

## 추가 설명

- **희귀 청크 우선 요청(rarest first)**: 네트워크 내에서 가장 부족한 청크를 먼저 확보하여, 전체 네트워크의 데이터 다양성을 유지하고 파일 분배 효율을 높임.    
- **Tit-for-tat 전송 정책**: “주는 만큼 받는다” 원칙. → 협력적인 피어가 더 많은 혜택을 받도록 설계되어, **무임승차(free-riding)** 문제를 방지함.
- **Optimistic unchoking**: 새로운 피어가 네트워크에 들어올 수 있는 기회를 주고, 더 나은 교환 상대를 찾기 위한 탐색 기능.


