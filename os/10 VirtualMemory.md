 ## 📘Background (Virtual Memory)

- **명령어는 물리 메모리에 있어야 실행 가능함**
  - → 그렇다면 전체 프로그램을 메모리에 올려야 할까?

- **일부 코드는 실행 중 거의 사용되지 않음**
  - 예외 처리 코드
  - 필요 이상으로 큰 배열/리스트
  - 자주 호출되지 않는 서브루틴 등

- **대안**
  - 프로그램의 일부만 메모리에 올리고 실행하는 방식
  - → 가상 메모리 개념의 필요성 제시

- **프로그램을 부분적으로 메모리에 올려 실행할 수 있다면** 다음과 같은 이점이 있음:
  - 물리 메모리 크기에 의해 프로그램 실행이 제한되지 않음
  - 동시에 더 많은 프로그램을 실행 가능
  - 프로그램을 로드하거나 스왑하는 데 필요한 I/O 감소


## 📘 Virtual Memory

- **가상 메모리(Virtual Memory)**: 사용자 논리 메모리와 물리 메모리의 분리
- 장점:
  - 프로그램 일부만 메모리에 올려도 실행 가능
  - 논리 주소 공간이 물리 주소 공간보다 훨씬 클 수 있음
  - 여러 프로세스가 주소 공간을 공유 가능
  - 프로세스 생성 효율 향상 (예: Copy-on-Write)
  - 동시에 더 많은 프로그램 실행 가능
  - 프로그램 로드·스왑 시 I/O 비용 감소
  - ![[Pasted image 20250513141000.png]]

> ⬅ 그림 설명: 논리 메모리는 물리 메모리 및 보조 저장장치와 매핑되어 있음


## 📘 Virtual Address Space

- **가상 주소 공간(Virtual Address Space)**: 프로세스가 메모리에 저장되는 논리적 구조
- 특징:
  - 일반적으로 주소 0부터 시작하여 연속적인 주소로 구성
  - 실제 물리 메모리는 페이지 프레임 단위로 구성됨
  - MMU(Memory Management Unit)가 논리 주소를 물리 주소에 매핑
  - 프로그래머는 메모리 관리 부담을 덜 수 있음
  - 가상 주소 공간은 **희소(sparse)**할 수 있음
    - 예: 스택/힙이 확장되거나 동적 라이브러리 연결 시 중간 공간이 채워짐
![[Pasted image 20250513141303.png]]

> ⬅ 그림 설명: 코드, 데이터, 힙, 스택 영역으로 구성된 전형적인 가상 주소 공간 구조


## 📘Page Sharing

- 가상 메모리는 **두 개 이상의 프로세스가 파일과 메모리를 공유**할 수 있도록 함
- 공유 방식:
  - 시스템 라이브러리를 가상 주소 공간에 매핑하여 공유
  - 공유 메모리를 읽기/쓰기 가능한 페이지로 매핑하여 가상 주소 공간에 배치
  - `fork()` 호출 시 페이지를 공유하여 **프로세스 생성 속도 향상**


## 📘 Demand Paging

- **Demand Paging**: 페이지는 **실제로 필요할 때만 메모리에 적재**
  - 프로그램 실행 중 요구된 시점에 로딩됨
- 장점:
  - 스와핑 기반 페이징과 유사하지만 **불필요한 I/O 없음**
  - **적은 메모리 사용량**
  - **빠른 응답 시간**
  - **더 많은 사용자 수용 가능**
- 필요 조건:
  - 페이지가 **메모리에 있는지 디스크에 있는지 구분할 수 있는 하드웨어 지원** 필요
    - (→ valid-invalid 비트를 통한 페이지 상태 구분)
![[Pasted image 20250513142552.png]]
> ⬅ 그림 설명:
> - 논리 메모리에서 A~H 페이지가 존재
> - 페이지 테이블은 각 페이지가 **어느 프레임에 있고 유효한지** 나타냄
> - 유효하지 않은 페이지는 **백업 저장소(backing store)**에 있음


## 📘Page Table with Valid-Invalid Bit

- **유효/무효 비트(valid/invalid bit)**는 각 페이지가 물리 메모리에 존재하는지 여부를 나타냄

### 🟢 Valid
- 해당 페이지가 **물리 메모리에 존재**
- 접근 시 **정상적으로 실행됨**

### 🔴 Invalid
- 해당 페이지가:
  - 프로세스의 유효한 논리 주소 공간에 **속하지 않거나**
  - 아직 물리 메모리에 **로드되지 않음**
- 접근 시 **페이지 폴트(page fault)** 발생 → OS가 처리


## 📘 Handling Page-Fault

- **페이지 폴트(Page Fault)** 처리 절차:

1. 내부 테이블(예: 페이지 테이블)을 확인하여 **참조가 유효한 주소인지 검사**
2. 주소가 **무효(invalid)**이면 → **프로세스 종료**
3. 주소가 유효하지만 아직 메모리에 없으면 → **해당 페이지를 메모리에 적재**

### 📌 페이지 적재 과정:
- 빈 프레임 찾기
- 원하는 페이지를 디스크에서 읽어와 그 프레임에 적재
- 페이지 테이블 수정 (valid 비트 갱신 등)
- 해당 명령어를 다시 시작 (재시도)
![[Pasted image 20250513143623.png]]
> ⬅ 그림 설명:
> 1. 프로세스가 유효하지 않은 페이지에 접근 시도
> 2. 트랩 발생 → OS로 제어 이동
> 3. 백업 저장소에서 해당 페이지 검색
> 4. 물리 메모리에 로딩
> 5. 페이지 테이블 수정
> 6. 중단된 명령어 재실행


## 📘 Aspects of Demand Paging

### 🔹 극단적인 경우 (Extreme Case)
- 프로세스를 **아무 페이지도 없이 시작**
- OS가 첫 명령어 위치로 IP 설정 → 페이지 폴트 발생
- 나머지 페이지들도 **접근 시마다 로드됨**
- → 이를 **Pure Demand Paging**이라 부름

### 🔹 하나의 명령어가 여러 페이지에 접근할 수도 있음
- 예: 메모리에서 두 값을 더하고 다시 저장하는 명령어
- → 이 경우 **여러 번의 페이지 폴트 발생 가능**
- 다행히 **참조 지역성(locality of reference)** 덕분에 실제로는 폴트 수 감소

### 🔹 Demand Paging에 필요한 하드웨어 지원
- 유효/무효 비트를 포함한 **페이지 테이블**
- **보조 기억 장치** (예: 스왑 공간)
- **명령어 재시작 기능**: 페이지 폴트 후 명령어 재실행


## 📘 Free-Frame List

- 페이지 폴트 발생 시, OS는 해당 페이지를 **보조 저장 장치에서 주기억 장치로 가져와야 함**
- 이를 위해 대부분의 운영체제는 **free-frame list (빈 프레임 목록)**을 유지함
- **Zero-fill-on-demand**:
  - 프레임을 할당하기 전에 내용을 0으로 초기화
- 시스템 부팅 시, 모든 사용 가능한 메모리는 free-frame list에 등록됨


## 📘Performance of Demand Paging

- **효과적인 접근 시간(Effective Access Time, EAT)**: EAT = (1 - p) * ma + p * page_fault_time
- `ma`: 메모리 접근 시간 (10~200 ns)
- `p`: 페이지 폴트 확률
- page_fault_time: 
  - 인터럽트 처리 (1~100 μs)
  - 디스크에서 페이지 읽기 (~8 ms)
  - 프로세스 재시작 (1~100 μs)
- **페이지 폴트율은 매우 낮게 유지되어야 함**


## 📘 Demand Paging Optimizations

- **스왑 공간 I/O**는 파일 시스템 I/O보다 빠름
  - 이유: 더 큰 단위로 할당되고 관리 오버헤드 적음

- 프로그램 실행 방식:
  1. 전체 파일을 스왑 공간으로 복사 후 실행
  2. 처음엔 파일 시스템에서 demand paging, 이후는 스왑 공간에서 처리

- **모바일 시스템**:
  - 스왑 지원 안함
  - 파일 시스템에서 demand page 후, 읽기 전용 페이지 재사용


## 📘 Copy-on-Write

- **fork()** 중복 페이지는 부모에게 속합니다
- **Copy-on-Write (COW)**:
  - 초기에는 복사가 아닌 공유
  - 초기에는 부모-자식이 같은 페이지를 **읽기 전용**으로 공유
  - 둘 중 하나가 해당 페이지에 **쓰기(write)** 시도 시, 그 순간 **실제 복사 발생**
  - → 프로세스 생성 속도 향상, 메모리 절약

- 추가 개념:
  - `vfork()`: 논리적으로 부모와 메모리를 공유하는 방법 (현재는 거의 사용 안 함)
  - 많은 OS는 COW나 힙/스택 확장을 위해 **Zero-fill-on-demand (ZFOD)** 프레임을 별도로 유지


## 📘 Two Major Problems in Demand Paging

- Demand Paging에서 주요 문제 두 가지:
  1. **Page Replacement Algorithm**
     - 페이지를 교체할 때 어떤 페이지를 제거할 것인가?
     - 초기 접근과 재접근 모두에서 **낮은 페이지 폴트율**이 이상적

  2. **Frame Allocation Algorithm**
     - 각 프로세스에 **얼마나 많은 프레임을 할당할 것인가?**
     - 어떤 프레임을 교체 대상으로 정할 것인가?

- 작은 개선만으로도 성능이 크게 향상될 수 있음


## 📘 Page Replacement 개요 

- 페이지 폴트 발생 시 **비어 있는 프레임이 없으면**, 사용 중이지 않은 프레임을 찾아 교체해야 함
- 페이지 교체 시, **dirty-bit(수정 비트)**를 사용하면 디스크 쓰기 비용 절감 가능


## 📘 Page Replacement 절차

1. 디스크에서 원하는 페이지의 위치 찾기  
2. 프레임 확보:
   - a. **빈 프레임**이 있다면 그대로 사용
   - b. 없으면 **페이지 교체 알고리즘**으로 희생(victim) 프레임 선택
   - c. 희생 프레임이 수정된 경우 디스크에 기록

3. 원하는 페이지를 확보한 프레임에 적재 → 페이지/프레임 테이블 갱신  
4. 폴트를 유발한 명령어 **재시작**

> 주의: 이 절차로 인해 **최대 2회의 페이지 전송** 발생 가능 → EAT 증가



## 📘 Page Faults vs. Number of Frames

- 일반적으로, **프레임 수가 많아질수록 페이지 폴트는 감소함**
- 하지만 모든 알고리즘에서 그런 것은 아님 → 예외 존재


## 📘 Page Replacement Algorithms 개요

- 다양한 페이지 교체 알고리즘 존재:
  - **FIFO (First-In, First-Out)**: 가장 오래된 페이지 제거
  - **Optimal (OPT)**: 미래에 가장 늦게 사용할 페이지 제거 (이론적 최적)
  - **LRU (Least Recently Used)**: 가장 오랫동안 사용하지 않은 페이지 제거
  - **LRU 근사 알고리즘**: 하드웨어 지원이 부족한 경우 사용
  - 기타 알고리즘 등

> ⬅ 이후 각 알고리즘별 예시와 비교 제시


## 📘 FIFO Page Replacement

- **FIFO (First-In, First-Out)**: 가장 먼저 들어온 페이지를 교체
- 장점: 구현 간단
- 단점: **Belady's Anomaly** 발생 가능

## 📘 Belady’s Anomaly

- **Belady’s Anomaly**: 프레임 수가 증가했음에도 불구하고 **페이지 폴트가 증가**하는 현상
- FIFO 알고리즘에서 대표적으로 발생함
- 이는 일부 교체 알고리즘이 비직관적인 동작을 할 수 있음을 시사


## 📘 Optimal Page Replacement

- **OPT (Optimal Replacement)**: 앞으로 **가장 오랫동안 사용되지 않을 페이지**를 교체
- 단점:
  - **미래 접근 정보가 필요**하므로 실질 구현은 불가능
  - 성능 비교를 위한 **기준(Benchmark)** 으로 사용됨


## 📘 LRU Page Replacement 

- **LRU (Least Recently Used)**: **가장 오랫동안 사용되지 않은 페이지**를 교체
- 장점:
  - 좋은 성능, 현실적으로 많이 사용됨
  - **참조 지역성(locality of reference)** 가정을 따름


## 📘 Implementation of LRU

### 방법 1: Counter 기반
- 각 페이지 테이블 항목에 **마지막 사용 시각(time-of-use)** 저장
- 페이지 접근 시 현재 시간으로 업데이트

### 방법 2: Stack 기반
- 최근 참조된 페이지를 **스택의 맨 위에** 유지
- 페이지 참조 시, 해당 페이지를 제거 후 맨 위에 삽입

> ⬅ 실제 구현 비용은 높음 → 근사 알고리즘이 많이 사용됨


## 📘 Stack Algorithm

- **Stack Algorithm**:
  - 프레임 수 `n`일 때 메모리에 있는 페이지 집합은,
  - `n+1`일 때의 집합에 **포함되는 부분 집합**

- 이 특성 덕분에 **Belady’s Anomaly가 발생하지 않음**
- **LRU는 Stack 알고리즘에 해당됨**


## 📘 LRU-Approximation Page Replacement

- **LRU는 이상적이지만, 정확한 구현이 어려움**
- 대부분 시스템은 **참조 비트(reference bit)**만 제공
  - 어떤 페이지가 **참조되었는지 여부**만 판단 가능
  - 참조된 **순서**는 알 수 없음
- 해결책: **LRU-approximation algorithms**
  - 추가 참조 비트 알고리즘
  - Second-chance 알고리즘 등
### 🔹 추가 참조 비트 알고리즘
- 각 페이지에 참조 비트(초기 0)
- 참조 시 1로 설정
- **참조 비트가 0인 페이지부터 제거**

### 🔹 Second-Chance 알고리즘
- **FIFO + 참조 비트**
- 교체 대상의 참조 비트:
  - `0` → 즉시 교체
  - `1` → 비트를 0으로 바꾸고 다음 페이지 확인

> 구현은 **시계 알고리즘 (Clock replacement)** 방식으로 수행


## 📘 Enhanced Second-Chance Algorithm

- **참조 비트 + 수정 비트(dirty bit)**를 함께 고려
- `(참조, 수정)` 쌍에 따라 우선순위 결정

| Reference | Modify | 의미                            | 우선순위 |
|-----------|--------|----------------------------------|----------|
| 0         | 0      | 최근에 사용 안됨 + 수정 안됨     | 가장 우선 |
| 0         | 1      | 최근에 사용 안됨 + 수정됨       | 다음 후보 |
| 1         | 0      | 최근에 사용됨 + 수정 안됨       | 유지 우선 |
| 1         | 1      | 최근에 사용됨 + 수정됨          | 유지 우선 |

- **시계 구조**로 4개의 클래스 중 가장 낮은 등급부터 탐색


## 📘 Counting Algorithms

- 각 페이지의 **참조 횟수**를 기록

### 🔹 LFU (Least Frequently Used)
- 참조 횟수가 가장 적은 페이지 교체

### 🔹 MFU (Most Frequently Used)
- 참조 횟수가 많은 페이지가 오래된 것일 수 있음 → 이를 우선 제거

- 단점: **구현 복잡도 높고**, 실제 시스템에서는 드물게 사용


## 📘 Page-Buffering Algorithms

- **항상 일정 수의 빈 프레임(pool of free frames)을 유지하여 페이지 폴트 지연 최소화**

### 🔹 주요 기법들

1. **사전 프레임 확보**  
   - 페이지 폴트 시 즉시 사용 가능한 프레임 확보

2. **희생(victim) 프레임 별도 저장**  
   - 새 페이지를 빈 프레임에 적재 후,
   - 기존 페이지는 나중에 제거 (비동기 처리)

3. **수정된 페이지 목록 유지**
   - 백업 저장소가 한가할 때 비동기적으로 기록

4. **프레임 내용을 보존하고 내용 기록**
   - 페이지가 다시 참조되면 디스크에서 불러오지 않아도 됨

> 목적: 교체 정책 오류로 인한 디스크 접근 비용을 줄이는 데 있음


## 📘 Allocation of Frames

- 시스템의 **고정된 프레임 수**를 각 프로세스에 어떻게 분배할 것인가?

### 🔹 고려사항:
- 각 프로세스는 최소한 몇 개의 프레임을 할당받아야 함
- 할당 프레임 수가 적어지면 → **페이지 폴트율 증가** → **성능 저하**
- 하나의 명령어가 참조할 수 있는 **모든 페이지를 담을 수 있을 만큼**은 최소로 필요


## 📘 Allocation Algorithms

### 🔹 Equal Allocation
- m개의 프레임을 n개의 프로세스에 **균등 분할**
- 각 프로세스는 `m/n`개의 프레임을 받음

### 🔹 Proportional Allocation
- 프로세스 크기에 비례하여 할당
- 계산식:  ai = (si / S) * m

- `ai`: 프로세스 i에 할당되는 프레임 수  
- `si`: 프로세스 i의 크기  
- `S`: 전체 프로세스 크기의 합


## 📘 Global vs. Local Allocation

### 🔹 Global Replacement
- **모든 프로세스의 프레임 중에서 희생 프레임 선택 가능**
- 한 프로세스의 페이지 폴트가 **다른 프로세스의 프레임**을 빼앗을 수 있음
- 단점: 프로세스가 자신의 페이지 폴트율을 제어하기 어려움

### 🔹 Local Replacement
- 각 프로세스는 **자기에게 할당된 프레임 내에서만 교체 수행**
- 다른 프로세스의 여유 프레임을 활용할 수 없음

> 실무에서는 **Global Replacement가 더 일반적**임


## 📘 Reclaiming Pages

- **전역 페이지 교체 정책(global replacement)**을 구현하기 위한 전략

### 🔹 핵심 개념
- 모든 메모리 요청은 **free-frame list**에서 처리
- 리스트가 **임계값 이하**로 떨어질 때 → 페이지 교체 수행
- 목표: 항상 **충분한 빈 메모리 프레임**을 확보하여 지연 없이 요청을 처리

> 즉, 메모리가 다 찬 뒤에 교체를 시작하는 것이 아니라 **미리미리 여유 공간을 확보**


## 📘 Non-Uniform Memory Access (NUMA)

- 전통적으로 모든 메모리는 **동일한 속도**로 접근 가능하다고 가정했으나,
- NUMA 시스템에서는 **CPU에 따라 메모리 접근 속도 차이 존재**

### 🔹 성능 최적화 방법
- **스레드가 실행되는 CPU 근처의 메모리**에 데이터 할당
- 스케줄러가 가능한 한 **동일한 시스템 보드에 스레드를 유지**

### 🔹 Solaris의 해결 방법: `lgroups`
- CPU와 메모리를 묶은 **저지연 그룹**을 구성
- 스케줄러와 페이지 매핑이 이를 활용하여 성능 최적화

> NUMA 환경에서는 스케줄링과 메모리 할당 모두 **위치 인식형**이어야 성능이 유지됨


## 📘 Thrashing

- **프로세스에 충분한 프레임이 할당되지 않으면** 페이지 폴트가 **빈번히 발생**

### 🔹 현상:
1. 페이지 폴트 발생 → 기존 프레임 교체
2. 교체된 프레임이 곧 다시 필요해짐
3. 다시 페이지 폴트 발생 → 반복

### 🔹 결과:
- CPU 사용률 저하
- OS는 멀티프로그래밍 수준을 **높이려는 잘못된 판단**을 함
- 더 많은 프로세스를 추가 → 상황 악화
- 즉, **스래싱(thrashing)**: 프로세스가 **계속해서 페이지 스와핑만 반복**하며 실질적인 작업을 하지 못하는 상태

## 📘 Demand Paging and Thrashing

- 스래싱을 방지하려면, **프로세스에 충분한 프레임 수 제공 필요**
- 문제: 얼마가 "충분한가"?

### 🔹 해결 방법: Locality Model
- **Locality**: 실행 중 함께 자주 참조되는 페이지들의 집합
- 대부분의 프로그램은 여러 개의 **지역성(localities)**으로 구성됨
- 각 프로세스가 **자신의 현재 locality만큼의 프레임**을 확보해야 안정적인 실행 가능


## 📘 Working-Set Model

- **Working Set**: 최근 Δ(델타)만큼의 페이지 참조에서 사용된 페이지들의 집합
  - Δ: Working-set window (페이지 참조 수 기준의 시간 구간)

### 🔹 개념:
- `WSSᵢ`: 프로세스 i의 working set 크기
- 프로세스 i는 실행을 위해 최소 `WSSᵢ`개의 프레임 필요
- 모든 프로세스의 WSS 합 > 시스템의 총 프레임 수 → **스래싱 발생**
![[Pasted image 20250513175946.png]]
> ⬅ 전체 메모리 요구량을 측정하고 스래싱을 방지하는 데 유용한 모델


## 📘 Working Sets and Page Fault Rates

- 프로세스의 **working set 변화는 페이지 폴트율과 밀접한 관련**이 있음
- Working set은 시간에 따라 **증가와 감소 반복**
- 이 변화는 **페이징 동작의 부하 예측 및 최적화**에 활용 가능


## 📘 Page-Fault Frequency

- **스래싱 방지를 위한 대안적 방법**: 페이지 폴트 빈도(Page-Fault Frequency, PFF) 제어

### 🔹 동작 방식:
- 프로세스의 PFF가 **너무 높음** → 프레임 **추가 할당**
- PFF가 **너무 낮음** → 프레임 **회수**
![[Pasted image 20250513180159.png]]

> ⬅ PFF 기반 제어는 동적으로 멀티프로그래밍 수준을 조절하는 데 사용됨


## 📘 Allocating Kernel Memory

- 커널 메모리 할당은 **특수한 처리 필요**
  - 다양한 크기의 자료구조에 메모리 요청
  - 커널 코드는 일반적으로 **페이지 교체 대상 아님**
  - 일부 하드웨어 장치는 **물리 메모리와 직접 연결**됨 → **연속된 물리 페이지 필요**

### 🔹 대표적인 커널 메모리 할당 기법:
- **Buddy System**
- **Slab Allocation**


## 📘 Buddy System
- **고정 크기의 연속된 메모리 세그먼트**에서 메모리 할당
- **2의 제곱 단위 크기**로 분할하여 할당
  - 예: 256KB 중 21KB 요청 → 가장 가까운 32KB 블록 제공

### 🔹 장점:
- 인접 블록을 쉽게 병합 가능 → 관리 용이

### 🔹 단점:
- **내부 단편화** 발생 가능'


## 📘 Slab Allocation

- 문제: 페이지 크기 단위 할당은 **요청 크기와 불일치**할 수 있음
- 해결: **Slab 구조 사용**
  - Slab: 하나 이상의 연속된 물리 페이지로 구성
  - 각 Slab은 특정 커널 자료구조용 **객체(Object)** 집합으로 구성

### 🔹 구조:
- **Cache**: 동일한 자료구조용 슬랩들의 집합
- 자료구조별 캐시 생성 (예: 프로세스 디스크립터, 파일 객체 등)

- 각 캐시는 해당 구조체 인스턴스(객체)로 채워짐
  - 객체 사용 전 → **free 상태**
  - 저장 시 → **used 상태**
  - Slab이 모두 사용되면 → 다음 Slab에서 할당

### 🔹 장점:
- **단편화 없음**
- **빠른 메모리 할당 및 해제**
- 자주 생성/삭제되는 구조체에 적합


## 📘 Prepaging

- **Pure Demand Paging의 문제점**: 초기에 많은 페이지 폴트 발생
- **Prepaging**: 필요할 것으로 예상되는 여러 페이지를 **한 번에 미리 적재**
  - 예: Working-Set Model 기반으로 미리 적재

### 🔹 고려사항:
- 미리 적재 비용 vs. 실제 페이지 폴트 처리 비용 비교 필요


## 📘 Page Size

- **페이지 크기에 따른 주요 특성 비교**:

| 항목             | 작은 페이지         | 큰 페이지            |
|------------------|---------------------|----------------------|
| 페이지 테이블 크기 | 큼                  | 작음                 |
| 메모리 활용 효율  | 좋음                | 나쁨 (내부 단편화)   |
| I/O 지연          | 큼                  | 작음                 |
| 지역성            | 좋음                | 나쁨                 |
| 페이지 폴트 수    | 많음                | 적음                 |

- 전반적인 추세: 하드웨어 성능 향상에 따라 **페이지 크기 증가 중**



## 📘 TLB Reach 

- **TLB Reach**: TLB로 접근 가능한 총 메모리 양

### 🔹 계산식: TLB Reach = TLB 엔트리 수 × 페이지 크기

- **TLB 성능 향상 방법**:
  - TLB 크기 증가 → 단점: 비용·전력 소모 큼
  - **페이지 크기 증가**로 reach 향상 가능

- 문제점: 페이지가 커지면 **단편화 증가**
- 일부 시스템은 **여러 크기의 페이지 지원**
  - S/W 관리 TLB (UltraSparc, MIPS, Alpha 등)
  - H/W 관리 TLB (PowerPC, Pentium 등)


## 📘 Inverted Page Tables

- **목표**: 페이지 테이블이 사용하는 물리 메모리 양 절감

### 🔹 구조:
- 하나의 전역 페이지 테이블만 유지 (프로세스당 아님)
- 항목 수 = 물리 프레임 수

### 🔹 단점:
- **논리 주소 공간 전체 정보를 포함하지 않음**
- **Demand Paging에 필요한 주소 정보가 부족**

→ 해결책: **외부 페이지 테이블**을 프로세스별로 따로 유지  
  (필요 시 디스크에서 페이징 인/아웃 가능)


## 📘 Program Structure

- 사용자는 메모리 구조를 몰라도 되지만, **알고 있으면 성능 향상 가능**

### 🔹 예시:

	int data[128][128];

- 행 우선 저장(메모리 상):
    
    - `data[i][j]`에서 **j가 먼저 증가해야 지역성 좋음**
        

### 🔹 두 코드 비교:

- 프로그램 1: `for (j) for (i)` → **16,384 page faults**
    
- 프로그램 2: `for (i) for (j)` → **128 page faults**

→ **올바른 순서의 반복문이 페이지 폴트 수에 큰 영향**

## 📘 I/O Interlock

- **I/O 수행 중인 페이지는 교체되면 안 됨**

### 🔹 이유:
- 예: 디스크로부터 파일을 읽어오는 중간에 해당 페이지가 교체되면 **I/O 오류 발생**

### 🔹 해결:
- **페이지 잠금(Pinning)** 수행
  - 해당 페이지를 **페이지 교체 대상에서 제외**시킴
  - → 안정적인 I/O 보장