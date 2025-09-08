

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


