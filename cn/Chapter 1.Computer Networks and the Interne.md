

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