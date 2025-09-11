

# 🌐 The Internet: a “nuts and bolts” view

## 1. Devices (컴퓨팅 장치)

- **Hosts = end systems**
- 인터넷의 **edge**에서 네트워크 앱 실행
- 예시: PC, 서버, 스마트폰, IoT 기기 등

## 2. Packet Switches (패킷 스위치)

- 데이터(패킷)를 전달하는 장치
- **Routers, Switches**

## 3. Communication Links (통신 링크)

- 매체: **fiber, copper, radio, satellite**  
- 성능: **bandwidth (전송 속도)**

## 4. Networks (네트워크)

- 장치, 라우터, 링크의 집합  
- 특정 조직이 관리

## 5. Internet 구조 예시

- **Home network** (가정 네트워크)
- **Enterprise network** (기업 네트워크)
- **Mobile network** (모바일 네트워크)
- **Local/Regional ISP** (지역 ISP)
- **National/Global ISP** (전국/글로벌 ISP)
- **Content provider network** (콘텐츠 제공 네트워크, 예: 데이터센터)

※ **ISP (Internet Service Provider)**: 인터넷 서비스 제공자

## 1. Internet: “network of networks”

- **네트워크의 네트워크**
- 상호 연결된 **ISPs (Internet Service Providers)**

## 2. Protocols (프로토콜)

- **everywhere (어디에나 존재)**
- 메시지 전송 및 수신을 제어
- 예시:

    - **HTTP** (웹)
    - **Streaming video** (스트리밍)
    - **Skype**
    - **TCP, IP**
    - **WiFi, 4G, Ethernet**

## 3. Internet Standards (인터넷 표준)

- **RFC (Request for Comments)**  
- **IETF (Internet Engineering Task Force)**

## 4. 그림 속 주요 연결

- **Home network** ↔ Skype   
- **Enterprise network** ↔ HTTP, Ethernet, WiFi
- **Mobile network** ↔ 4G
- **Content provider network / Datacenter** ↔ Streaming video, TCP
- **National/Global ISP** ↔ IP



# 🌐 The Internet: a “service” view

## 1. Infrastructure (인프라)

- 애플리케이션에 **서비스를 제공**
- 예시:
    - Web
    - Streaming video
    - Multimedia teleconferencing
    - Email
    - Games
    - E-commerce
    - Social media
    - Interconnected appliances (IoT 등)

## 2. Programming Interface (프로그래밍 인터페이스)

- 분산 애플리케이션을 위한 **연결 지점 제공**
- **Hooks**: 앱이 인터넷 전송 서비스를 **연결·사용**하도록 허용
- 다양한 서비스 옵션 제공 (우편 서비스와 유사한 개념)

## 3. 그림 속 예시

- **Skype** (Home network ↔ Mobile network)
- **HTTP** (Enterprise network ↔ Content provider network)
- **Streaming video** (Datacenter ↔ ISP ↔ Home/Mobile)


# 📡 What’s a protocol?

## 1. Human Protocols (인간 프로토콜)

- “What’s the time?”
- “I have a question”
- Introductions (소개)
- 특징:
    - 특정 메시지가 전송됨
    - 메시지를 받거나 이벤트 발생 시 특정 행동 수행


## 2. Network Protocols (네트워크 프로토콜)

- 주체: **컴퓨터(장치)**   
- 모든 인터넷 통신 활동은 프로토콜에 의해 규제됨

## 3. Definition (정의)

> **Protocols define the format, order of messages sent and received among network entities, and actions taken on message transmission, receipt.**

프로토콜은 **네트워크 엔티티 간 주고받는 메시지의 형식과 순서**, 그리고 **메시지 전송·수신 시 수행되는 동작**을 정의한다.


## A closer look at Internet structure — _Network edge_

- **호스트(클라이언트·서버)**가 네트워크 가장자리(엣지)를 구성
- **서버**는 주로 **데이터센터**에 위치


## _Network edge + Access networks_

- 엣지: **호스트(클라이언트·서버)**, 서버는 **데이터센터**에 위치
- **액세스 네트워크·전송매체**: 유선·무선 통신 링크로 구성


## _Network core_

- **네트워크 코어**: 상호 연결된 **라우터들**이 이루는 **network of networks**


## _개요_

- **질문**: 엣지 라우터까지 **종단 시스템을 어떻게 연결**하는가?
    - **가정용**, **기관(학교/기업)**, **모바일(예: Wi-Fi, 4G/5G)** 액세스
- 확인 포인트: **전송률(bps)**, **공유/전용 접속 여부**.


## Access networks — _Cable

- **주파수분할다중화(FDM)**: 서로 다른 **주파수 대역**으로 채널을 분리하여 동시에 전송함
- **동축(Coax)** 기반 분배망에서 TV·데이터 채널을 **주파수대역**으로 구분함
- **다운스트림** 최대 **40 Mbps–1.2 Gbps**, **업스트림** **30–100 Mbps**(비대칭)
- **HFC(Hybrid Fiber Coax)** 구조, 가정들은 **케이블 헤드엔드**까지 **공유형**(isp) 액세스망을 사용함


## Access networks — _DSL_

- **전화선 그대로** 쓰면서, **음성·데이터를 주파수로 분리**해 동시에 보냅니다.
- 집 ↔ 전화국 **전용선**(중앙국의 **DSLAM**에 연결)입니다.
- **다운 24–52 Mbps, 업 3.5–16 Mbps**(대칭 아님).


## FTTH

- 집에서 **중앙국까지를 광섬유**로 연결합니다.
- **기가비트급 잠재 전송률**(실제는 수십~수백 Mbps 수준).
- **스플리터–OLT/ONT** 구조(집→스플리터 구간은 전용, 스플리터→OLT는 공유).


## 가정용 네트워크(홈 네트워크)

- **케이블/DSL 모뎀 → 라우터·방화벽·NAT → 유선(1 Gbps)/Wi-Fi(54, 450 Mbps)** 구성입니다.
- 이들이 **한 박스**에 합쳐진 제품도 흔합니다



# 무선 접속 네트워크 (Wireless Access Networks)

## 개요

- **공유 무선 네트워크**가 종단 시스템(PC, 스마트폰 등)을 **라우터**에 연결합니다.
- 연결은 **기지국(Access Point)**을 통해 이루어집니다.

## 1. 무선 근거리 네트워크 (WLANs, Wi-Fi)

- 보통 **건물 내부나 주변 (~30m, 약 100ft)**에서 사용됩니다.
- **Wi-Fi 표준 (IEEE 802.11 b/g/n/ac/ax)**
    - 전송 속도: **11, 54, 450, 866, 2400 Mbps**
- 가정·사무실·카페 등에서 흔히 사용됩니다.

## 2. 광역 셀룰러 네트워크 (Wide-area Cellular Access)

- **이동통신사**가 제공하는 광역 네트워크 (수십 km 범위).  
- 속도: **수십~수백 Mbps**.
- 현재 **4G** 사용, **5G**가 도입되고 있습니다.

👉 요약:

- **Wi-Fi**는 가까운 거리(집·사무실).   
- **셀룰러(4G/5G)**는 넓은 거리(도시·광역).


# 기업/기관 네트워크 (Enterprise Networks)

## 개요

- **기업, 학교, 연구소** 같은 조직에서 사용하는 네트워크입니다.
- **유선 + 무선** 기술을 섞어 사용합니다.
- 여러 **스위치와 라우터**가 연결되어 있습니다.

## 구성 요소

- **Ethernet(이더넷)**:  
    - 유선 연결
    - 속도: **100 Mbps, 1 Gbps, 10 Gbps**
- **Wi-Fi**
    - 무선 연결
    - 속도: **11, 54, 450 Mbps**

## 특징

- **기관용 메일 서버, 웹 서버**도 내부 네트워크에 연결됩니다.
- 외부 인터넷은 **기관 라우터 → ISP**를 통해 연결됩니다.

👉 요약:

- **유선(Ethernet)**은 빠르고 안정적
- **무선(Wi-Fi)**은 편리하지만 속도는 상대적으로 낮음
- 둘을 함께 사용해 **융합된 네트워크**를 구축합니다.


# 링크: 물리 매체 (Physical Media)

## 기본 개념

- **bit**: 송신기 ↔ 수신기 사이를 전달되는 정보 단위.
- **physical link**: 송신기와 수신기 사이 실제 물리적 경로.    

## 매체 종류

- **Guided media (유도 매체)**
    - 신호가 **고체 매체**를 따라 전파됨.
    - 예: **구리선, 광섬유, 동축 케이블**.
- **Unguided media (비유도 매체)**
    - 신호가 **자유롭게 전파**됨.        
    - 예: **라디오(무선)**.

## 트위스티드 페어 (Twisted Pair, TP)

- 절연된 구리선 2개를 꼬아 만든 케이블.
- **Category 5 (Cat5)**: 100 Mbps, 1 Gbps Ethernet. 
- **Category 6 (Cat6)**: 10 Gbps Ethernet.

👉 요약:

- **Guided = 선을 통해** (구리, 광섬유).
- **Unguided = 공기 중** (무선).
- 가장 흔한 유도 매체 = **트위스티드 페어 케이블**.


# Links: Physical Media

## 1. Coaxial Cable (동축 케이블)

- **구조**: 두 개의 동심 구리 도체
- **특징**:
    - 양방향 통신 가능 (bidirectional)
    - 광대역(broadband) 지원
        - 케이블 내 다중 주파수 채널 가능
        - 채널당 수백 Mbps 속도

## 2. Fiber Optic Cable (광섬유 케이블)

- **구조**: 빛 펄스를 전달하는 유리 섬유 (각 펄스는 비트를 나타냄)   
- **특징**:
    - 고속 동작
        - 고속 지점 간(point-to-point) 전송: 10~100Gbps
    - 낮은 오류율
        - 중계기(repeater) 간격이 넓음
        - 전자기 잡음에 영향을 받지 않음 (면역성)



# Links: Physical Media (무선 전송 매체)

## 1. Wireless Radio (무선 라디오)
- **특징**:
    - 전자기 스펙트럼을 이용하여 신호 전달
    - 물리적인 “wire”가 없음
    - 방송(broadcast) 및 반이중(half-duplex) 통신 방식  
        (송신기 → 수신기)
- **전파 환경의 영향**:
    - 반사(reflection)
    - 물체에 의한 차단(obstruction)
    - 간섭(interference)
## 2. Radio Link Types (무선 링크 유형)

### (1) Terrestrial Microwave (지상 마이크로파)

- 최대 45 Mbps 채널 속도  
### (2) Wireless LAN (Wi-Fi)

- 최대 수백 Mbps 속도 (up to 100’s Mbps)
### (3) Wide-area (광역망, 예: Cellular)

- 4G 셀룰러: 약 수백 Mbps
### (4) Satellite (위성 통신)

- 채널당 최대 수백 Mbps
- 전송 지연(end-to-end delay): 약 270 ms
- 궤도 종류: 정지궤도(geosynchronous) vs 저궤도(low-earth-orbit)


# The Network Core

## 1. 구조

- **Mesh of interconnected routers**  
    여러 개의 라우터가 상호 연결된 메시(mesh) 구조로 구성됨.

## 2. Packet-Switching (패킷 교환)

- **개념**: 호스트(host)는 응용 계층 메시지를 잘라 **패킷(packets)** 으로 나눔.
- **동작 방식**:
    - 각 패킷은 **소스에서 목적지까지** 이동하는 동안 여러 라우터를 거쳐 전달됨.
    - 한 라우터에서 다음 라우터로 순차적으로 전달(forwarding).
    - 각 패킷은 **링크 용량 전체**를 사용하여 전송됨.

## 3. 주요 구성 요소

- **Local / Regional ISP**  
    지역 또는 중소 규모 인터넷 서비스 제공자
- **National / Global ISP**  
    국가 단위 또는 글로벌 규모 인터넷 서비스 제공자
- **기타 네트워크**
    - Home Network
    - Enterprise Network
    - Content Provider Network
    - Datacenter Network


# Host: Sends Packets of Data

## 1. Host Sending Function

- **과정**:
    - 응용 계층(application layer) 메시지를 받음
    - 메시지를 작은 단위로 분할 → **패킷(packets)** (길이 = _L bits_)
    - 패킷을 접속 네트워크(access network)에 전송  
        전송 속도 = **Transmission rate (R)**

## 2. Transmission Rate (전송 속도)

- **정의**: 링크 전송 속도 (link transmission rate)   
- 다른 명칭:
    - **Link capacity**
    - **Link bandwidth**

## 3. Packet Transmission Delay (패킷 전송 지연)

- **정의**: L-bit 패킷을 링크에 전송하는 데 필요한 시간   
- **공식**:
    Packet Transmission Delay=L (bits)/ R (bits/sec)

## 4. 요약

- 패킷 크기 (_L_)가 클수록 전송 지연 ↑   
- 링크 전송 속도 (_R_)가 높을수록 전송 지연 ↓


# Packet-Switching: Store-and-Forward

## 1. Transmission Delay (전송 지연)

- 정의: 길이 _L_ (bits)인 패킷을 전송 속도 _R_ (bps)로 링크에 밀어 넣는 데 걸리는 시간
## 2. Store and Forward (저장 및 전달)

- **라우터 동작 방식**:   
    - 전체 패킷이 라우터에 도착해야 다음 링크로 전송 가능
    - 부분적으로 도착한 패킷은 전송되지 않음

## 3. End-to-End Delay (종단 간 지연)

- 정의: 여러 홉(hop)을 거칠 때 누적되는 지연   
- 2홉 상황에서:
    End-to-End Delay=2L/R​
    (전파 지연이 0이라고 가정)

## 4. Numerical Example (수치 예시: One-Hop)

- L=10 Kbits  
- R=100 Mbps
- One-hop 전송 지연:
    10,000 / 100×10^6=0.1 msec


# Packet-Switching: Queueing Delay, Loss

## 1. Packet Queueing and Loss

- **조건**: 링크로 들어오는 데이터 도착률(throughput, bps)이 링크 전송 속도(transmission rate, bps)를 초과할 경우
- **결과**:
    - 패킷은 전송을 기다리며 **큐(queue)** 에 쌓임
    - 라우터의 메모리(버퍼)가 가득 차면 **패킷이 삭제(dropped, lost)** 될 수 있음

## 2. Queueing Delay (큐잉 지연)

- 출력 링크(output link)가 처리할 수 있는 속도보다 입력 패킷이 많을 때 발생   
- 전송 대기 시간 증가

## 3. Packet Loss (패킷 손실)

- 라우터의 **버퍼 공간 한계**를 초과할 경우 발생   
- 전송되지 못한 패킷은 폐기(dropped)됨

## 4. 그림 설명

- **입력 링크**: 100 Mb/s   
- **출력 링크**: 1.5 Mb/s
- 입력 속도가 출력 속도를 초과하므로 큐가 형성됨
- 큐가 꽉 차면 일부 패킷이 손실됨


# Two Key Network-Core Functions

## 1. Forwarding (포워딩)

- **정의**: 패킷을 **라우터 입력 링크(input link)** 에서 **적절한 출력 링크(output link)** 로 이동시키는 동작
- **특징**:
    - **Local action** (개별 라우터 내부 동작)
    - **포워딩 테이블(local forwarding table)** 참조
        - 헤더 값(header value)에 따라 출력 링크 결정    
- **예시**:
    - Header value = `0111` → Output link = 2        

## 2. Routing (라우팅)

- **정의**: 소스에서 목적지까지 패킷이 이동하는 전체 경로(path)를 결정하는 과정
- **특징**:
    - **Global action** (네트워크 전체 관점에서 수행)
    - 라우팅 알고리즘(routing algorithms)에 의해 결정됨
- **역할**: 라우터의 **포워딩 테이블 생성/업데이트**

|구분|Forwarding|Routing|
|---|---|---|
|범위|Local (개별 라우터 내부)|Global (네트워크 전체)|
|기능|패킷을 입력 링크 → 출력 링크로 전달|소스~목적지 경로 결정|
|기초 자료|Forwarding table|Routing algorithm 결과|

# Alternative to Packet Switching: Circuit Switching

## 1. 기본 개념

- **End-to-end 자원 할당**: 소스와 목적지 간의 “call” 동안 회선이 **예약(reserved)** 됨
- 패킷 교환과 달리 **전용 자원(dedicated resources)** 사용 → 공유 불가

## 2. 동작 방식

- 다이어그램 예시:
    - 각 링크는 4개의 회선을 가짐
    - 하나의 호출(call)이 **top 링크의 2번째 회선**과 **right 링크의 1번째 회선**을 할당받음
- **특징**:
    - 회선 기반(circuit-like) 성능 → **보장된 성능**
    - 호출이 사용하지 않으면 회선 구간은 **유휴(idle)** 상태 (자원 낭비)
    - **No sharing**: 다른 사용자와 공유 불가

## 3. 활용

- 전통적인 전화망(traditional telephone networks)에서 주로 사용됨  

## 4. Packet Switching vs Circuit Switching (비교 요약)
|구분|Packet Switching|Circuit Switching|
|---|---|---|
|자원 활용|공유 (동적 할당)|전용 (사전 예약)|
|효율성|유휴 시간 거의 없음|유휴 자원 발생 가능|
|성능|혼잡 시 지연/손실 발생|회선 품질 보장|
|활용 예|인터넷 데이터 통신|전통 전화망|


# Circuit Switching: FDM and TDM

## 1. Frequency Division Multiplexing (FDM)

- **개념**: 광(optical), 전자기 주파수 대역을 (좁은) 주파수 밴드로 분할
- **특징**:
    - 각 호출(call) → 자신의 밴드 할당
    - 해당 밴드 내에서 최대 속도로 전송 가능
- **시각화**: 여러 사용자가 **동시에**, 서로 다른 주파수 대역을 사용

## 2. Time Division Multiplexing (TDM)

- **개념**: 시간을 슬롯(slot)으로 분할   
- **특징**:
    - 각 호출(call) → 주기적으로 반복되는 슬롯 할당
    - (넓은) 주파수 대역을 사용할 수 있으나, **자신의 슬롯 시간 동안만 전송 가능**
- **시각화**: 여러 사용자가 **시간을 나눠서** 번갈아 전송

## 3. 비교 요약
|구분|FDM|TDM|
|---|---|---|
|자원 분할 기준|주파수 (Frequency)|시간 (Time)|
|사용자 동작|동시에 전송 (서로 다른 대역)|번갈아 전송 (시간 슬롯)|
|성능|각자 좁은 대역폭 보장|전체 대역폭 활용 가능하지만 시간 제한|
|활용 예|라디오/TV 방송, 전통 전화망|디지털 회선, 위성통신, 4G/5G 일부 방식|


# Packet Switching vs Circuit Switching

## 1. 핵심 비교

- **Packet Switching**: 더 많은 사용자가 네트워크를 동시에 활용 가능
- **Circuit Switching**: 사용자 수가 회선 용량에 제한됨

## 2. 예제 시나리오

- 링크 용량: **1 Gb/s**   
- 각 사용자:
    - 활성 시(active): **100 Mb/s 사용**
    - 활성 확률: **10%**

## 3. Circuit Switching

- 최대 **10명 사용자** 수용 가능 (1 Gb/s ÷ 100 Mb/s)   

## 4. Packet Switching

- **35명 사용자** 연결 가능  
- 동시에 10명 이상이 활성화될 확률 < **0.0004**
- 따라서 **자원 공유 기반**으로 더 많은 사용자 수용 가능

## 5. 비교 요약

|구분|Circuit Switching|Packet Switching|
|---|---|---|
|자원 할당|고정된 회선 할당|공유 기반 (동적 할당)|
|사용자 수|제한적 (회선 수로 제한)|더 많은 사용자 수용|
|성능|보장된 대역폭|혼잡 시 지연/손실 가능|
|활용 예|전통 전화망|인터넷 데이터 통신|



# Packet Switching vs Circuit Switching (심화 비교)

## 1. Packet Switching 장점

- **Bursty 데이터**에 적합
    - 어떤 순간에는 데이터 전송, 다른 순간에는 없음
- **자원 공유** (resource sharing)
- **단순성**: 별도의 call setup 불필요

## 2. Packet Switching 단점

- **혼잡(congestion) 발생 가능성**:  
    - 버퍼 오버플로우(buffer overflow)로 인한 지연 및 손실
- **신뢰성 확보 필요**:
    - 데이터 전송의 신뢰성을 위한 **전송 프로토콜 필요**
    - 혼잡 제어(congestion control) 필요

## 3. Circuit-like Behavior 필요성

- 오디오/비디오 같은 실시간 애플리케이션 → **대역폭 보장** 필요   
- 따라서, 회선 교환과 같은 성능 보장을 **패킷 교환 위에서 구현**하는 방법 연구됨  
    (예: QoS, 대역폭 예약 등)

## 4. 추가 질문 (Q)

- **인간 사회적 비유**:   
    - **Circuit Switching** → 예약된 자원 (예: 식당 좌석 예약)
    - **Packet Switching** → 필요할 때만 할당 (예: 무작위로 식당 방문 후 자리 차지)



# Internet Structure: A "Network of Networks"

## 1. Host 연결 방식

- 호스트(host)는 **Access ISP**(Internet Service Provider)를 통해 인터넷에 접속
- Access ISP 종류:
    - Residential (가정용 ISP)
    - Enterprise (기업, 대학교, 상업적 ISP 등)

## 2. ISP 상호 연결

- Access ISP는 반드시 서로 연결되어야 함   
- 목적: **임의의 두 호스트 간 패킷 전송 가능**

## 3. 네트워크의 복잡성

- 결과적으로 인터넷은 **네트워크들의 네트워크(network of networks)**   
- 구조 진화 요인:
    - **경제적 요인 (economics)**
    - **국가 정책 (national policies)**

## 4. 접근 방법

- 현재 인터넷 구조를 설명하기 위해 **단계적(stepwise) 접근** 필요


# Internet Structure: A "Network of Networks" (36~44)

## 36. Internet structure: a “network of networks”

- **문제 제기**: 수백만 개의 Access ISP를 어떻게 연결할 것인가?
- **직접 연결 문제**:
- 각 Access ISP를 서로 직접 연결하면 **O(N²) 연결** 필요
- 비현실적 (scalability issue)

- **대안 1**: 모든 Access ISP를 **하나의 글로벌 Transit ISP**에 연결
- 고객 ISP ↔ 글로벌 ISP 간 **경제적 계약(economic agreement)** 존재

- 하지만 **하나의 글로벌 ISP**만 존재한다면,  
  → 경쟁자(competitors)가 등장하게 됨

- 글로벌 ISP가 여러 개 존재한다면?  
    → 서로 연결 필요
- **IXP (Internet Exchange Point)**, **Peering link** 등장
    - ISP 간 상호 연결을 가능하게 함


- **지역 ISP (Regional ISP)** 등장
    - Access ISP를 묶어 상위 ISP와 연결
- 계층적 구조로 확장

**콘텐츠 제공자 네트워크(Content Provider Network)** 등장

- Google, Microsoft, Akamai 등
- 자체 네트워크를 운영하여 사용자 가까이에 서비스/콘텐츠 제공

**Tier 구조**:

- 소수의 대형 **Tier-1 ISP**가 중심 (예: Level 3, Sprint, AT&T, NTT)
    - 국가 및 국제 커버리지
- **콘텐츠 제공자 네트워크**:
    - Google, Facebook 등
    - 자체 데이터센터 네트워크를 운영, Tier-1/Regional ISP를 우회하기도 함


# How Do Packet Loss and Delay Occur?

## 1. Queueing in Router Buffers

- 패킷은 라우터의 **버퍼(buffer)** 에 저장됨
- **대기열(queue)** 에 쌓여 순서를 기다림

## 2. Delay 발생 원인

- **Transmission delay**: 패킷이 전송되는 동안 발생   
- **Queueing delay**: 출력 링크가 혼잡할 때, 패킷이 버퍼에서 대기하는 시간

## 3. Packet Loss 발생 원인

- 입력 패킷 도착률이 **출력 링크 용량(output link capacity)** 을 초과하면 버퍼가 가득 참   
- **버퍼 여유 공간이 없을 경우 → 패킷 폐기(loss)** 발생

## 4. 그림 설명

- A, B 호스트가 동시에 패킷을 전송  
- 라우터 버퍼에 패킷이 쌓임 (queue 형성)
- 일부는 전송 중 (transmission delay), 일부는 대기 (queueing delay)
- 버퍼가 꽉 차면 → 새 패킷은 drop(loss)


# Packet Delay: Four Sources

## 1. 총 지연 공식

![[Pasted image 20250908110307.png]]

## 2. 지연 요소

### (1) Processing Delay (dproc)

- **노드 처리 지연**
- 기능:
    - 비트 오류 검사 (check bit errors)
    - 출력 링크 결정 (determine output link)
- 일반적으로 < 1ms

### (2) Queueing Delay (dqueue​)

- **큐잉 지연**   
- 출력 링크에서 전송을 기다리는 시간
- 라우터의 혼잡 정도(congestion level)에 따라 달라짐

### (3) Transmission Delay (dtrans)

- 패킷을 링크에 밀어 넣는 시간  
- 계산식: L/R​ (패킷 길이 / 링크 전송 속도)


### (4) Propagation Delay (dprop)

- 신호가 물리 매체를 따라 전송되는 시간   
- 계산식: 
- 변수: dprop =d / s
- ddd: 링크 길이 (meters)
- sss: 전파 속도 (~ 2×10^8 m/sec, 광섬유 내 빛의 속도)

## 3. 요약 표
|Delay 종류|의미|주요 요인|크기|
|---|---|---|---|
|Processing|라우터 내부 처리|오류 검사, 라우팅|< 1 ms|
|Queueing|큐에서 대기|혼잡 수준|가변적|
|Transmission|링크에 패킷 밀어 넣는 시간|패킷 길이, 대역폭|L/R|
|Propagation|전파 지연|거리, 매체 속도|거리/속도|
## 3. 비교: dtrans vs dprop

- 두 지연은 성격이 크게 다름
    - dtrans: **패킷 크기와 대역폭**에 따라 달라짐
    - dprop​: **거리와 매체 속도**에 따라 달라짐
- 따라서 네트워크 성능 분석 시 구분해서 고려해야 함


# Caravan Analogy (전송 지연과 전파 지연 비유)

## 1. 시나리오

- 자동차 행렬(caravan) = **패킷**
- 자동차(car) = **비트(bit)**
- 속도: 100 km/hr (전파 속도와 유사)
- 톨게이트(toll booth) = **라우터**
    - 한 차량을 처리하는 데 12초 소요 (비트 전송 시간과 유사)

## 2. 계산 과정

- 차량 수 = 10대 (10-bit 패킷 비유)    

1. **톨게이트 처리 시간**
    12×10=120 sec
    → 전체 행렬이 톨게이트를 통과하는 데 걸리는 시간
2. **마지막 차량 전파 시간**
    - 거리: 100 km
    - 속도: 100 km/hr
    
    100/100=1 hour
1. **총 시간**
    - 톨게이트 처리(120초 = 2분) + 이동(60분)
    
    =62 minutes

## 3. 비유의 의미

- **Transmission delay**: 톨게이트에서 차량을 밀어 넣는 시간   
- **Propagation delay**: 도로를 따라 차량이 실제로 이동하는 시간
- 네트워크 지연을 이해하기 위해 자동차와 도로 시스템을 활용한 비유적 설명


# Caravan Analogy (확장 시나리오)

## 1. 새로운 조건

- 자동차(car) = 비트(bit), 행렬(caravan) = 패킷(packet)
- 자동차 전파 속도: **1000 km/hr**
- 톨게이트(toll booth) 처리 속도: **차량당 1분**

## 2. 질문 (Q)

- **Q**: 모든 차량이 첫 번째 톨게이트를 다 통과하기 전에, 일부 차량이 두 번째 톨게이트에 도착할 수 있을까    

## 3. 계산

- 첫 번째 차가 100 km 이동하는 데 걸리는 시간:   
    100/1000 hr= 0.1 hr =6 분
- 첫 번째 차는 1분 동안 톨게이트에서 처리됨 → 7분 후 도착
- 이때 아직 뒤의 3대는 첫 번째 톨게이트 처리 대기 중

## 4. 답변 (A)

- **Yes!**  
- 7분 후, 첫 번째 차량은 이미 두 번째 톨게이트에 도착
- 하지만 첫 번째 톨게이트에서는 여전히 3대가 처리 중    

## 5. 비유의 의미

- 네트워크에서 **전송 속도 증가**와 **처리 시간 증가**는 서로 다른 영향을 미침   
- 전파 속도가 충분히 빠르면, **모든 패킷이 송신 완료되기 전에 일부는 이미 다음 노드에 도착**할 수 있음


# Packet Queueing Delay (revisited)

## 용어 정의
| 기호  | 의미        | 단위        |
| --- | --------- | --------- |
| R   | 링크 대역폭    | bps       |
| L   | 패킷 길이     | bits      |
| α   | 평균 패킷 도착률 | packets/s |
## 트래픽 강도(traffic intensity)

ρ  =  L α/R​

- ρ는 링크가 처리해야 할 평균 작업량 대비 서비스 능력을 나타내는 지표입니다.

## 평균 큐잉 지연의 거동

- ρ≈0: 평균 큐잉 지연이 작습니다.
- ρ→1: 평균 큐잉 지연이 매우 커집니다(급격히 증가).
- ρ>1: 도착하는 작업량이 서비스 가능한 양을 초과합니다.
    - **무한 큐**: 지연이 **무한대**가 됩니다.
    - **유한 큐**: **손실(loss)** 이 매우 커집니다.

> [!important] 설계 원칙  
> 시스템의 트래픽 강도 ρ=LαR\rho=\dfrac{L\alpha}{R}ρ=RLα​ 가 **1을 초과하지 않도록** 설계합니다.


