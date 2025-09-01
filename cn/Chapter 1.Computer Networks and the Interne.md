

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

