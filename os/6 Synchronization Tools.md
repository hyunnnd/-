
# Producer/Consumer Problem

## 개요
- **Producer**: 데이터를 생성하고 이를 버퍼에 저장하고자 함
- **Consumer**: 버퍼에 있는 데이터를 꺼내어 사용함

## 구성 요소
- **Buffer (Shared Memory)**: Producer와 Consumer 간 공유되는 메모리 공간
  - 일반적으로 **circular queue**(원형 큐)로 구현

## 동작 방식
```text
Producer → [info.] → Buffer (Shared Memory) → [info.] → Consumer
```

## 예시: 다중 스레드 웹 서버

- **Producer**: HTTP 요청을 작업 큐에 넣음
- **Consumer**: 작업 큐에서 요청을 꺼내어 처리하는 스레드들



# Concurrent Access of Shared Data

## Producer 코드
```c
while (true) {
    while (counter == BUFFER_SIZE);
    buffer[in] = nextProduced;
    in = (in + 1) % BUFFERSIZE;
    counter++;  // 공유 자원 변경
}
```
## Consumer 코드
```c
while (true) {
    while (counter == 0);
    nextConsumed = buffer[out];
    out = (out + 1) % BUFFER_SIZE;
    counter--;  // 공유 자원 변경
}

```


## 문제 상황

- `counter` 변수는 **공유 자원(shared variable)**이며, **동시에 접근**됨
- 초기값이 5인 상황에서:
    - Producer는 `counter++` 수행
    - Consumer는 `counter--` 수행
- 이상적으로는 결과가 5로 유지되어야 하지만, **경쟁 조건(race condition)** 발생 시 정확한 결과 보장 불가



# 동기화 문제 (Synchronization Problem)

### `counter++` 구현
register₁ = counter  
register₁ = register₁ + 1  
counter = register₁

### `counter--` 구현
register₂ = counter  
register₂ = register₂ - 1  
counter = register₂

### 문제 상황: 명령어가 섞여 실행될 경우 (interleaved execution)

| Producer                              | Consumer                              |
| ------------------------------------- | ------------------------------------- |
| register₁ = counter *(register₁ = 5)* | register₂ = counter *(register₂ = 5)* |
| register₁ = register₁ + 1 *(= 6)*     | register₂ = register₂ - 1 *(= 4)*     |
| counter = register₁ *(counter = 6)*   | counter = register₂ *(counter = 4)*   |

> **counter can be 4 or 6 !**


# 동기화 문제 (Synchronization Problem)

## Race condition (경쟁 조건)
- 여러 프로세스가 동시에 동일한 데이터를 접근 및 조작하는 상황
- 실행 결과가 접근이 일어나는 **순서**에 따라 달라짐

## Synchronization (동기화)
- 사건들의 발생을 시간에 맞춰 **일관되게 동작하도록 조정**하는 것
- 한 번에 **오직 하나의 프로세스만 공유 데이터에 접근**하도록 보장


# 임계 구역 (Critical Section)

Critical section problem은 프로세스가 데이터를 협력적으로 공유할 수 있도록 활동을 동기화하는 데 사용할 수 있는 프로토콜을 설계하는 것입니다
## 개요
- n개의 프로세스 집합 {p₀, p₁, ..., pₙ₋₁}를 고려
- 각 프로세스는 **임계 구역(Critical Section)**이라는 코드 영역을 가짐

## 임계 구역의 특징
- 해당 구역에서는 다음과 같은 작업 수행 가능:
  - 공용 변수 수정
  - 테이블 갱신
  - 파일 쓰기 등
- 한 프로세스가 임계 구역에 들어가면, **다른 어떤 프로세스도 동시에 해당 임계 구역에 들어가서는 안 됨**

## 프로세스 흐름
- **Entry Section** → **Critical Section** → **Exit Section** → **Remainder Section**

> 공용 데이터 접근 시 반드시 임계 구역 보호 필요

## 각 구역 설명

- **Entry section**: 임계 구역 진입을 요청하는 코드 영역
- **Critical section**: 공유 자원(공용 변수, 파일 등)을 변경할 수 있는 코드 영역
- **Exit section**: 임계 구역에서 나왔음을 알리는 코드 영역
- **Remainder section**: 공유 자원을 변경하지 않는 나머지 코드 영역



# 운영체제에서의 임계 구역 처리 (Critical-Section Handling in OS)

## 예시: PID 할당 시 발생하는 경쟁 조건 (Race Condition)

- 프로세스 **P₀**와 **P₁**이 `fork()` 시스템 콜을 통해 자식 프로세스를 생성함
- 이때 커널 변수인 `next_available_pid`에 경쟁 조건이 발생함  
  → 이 변수는 다음에 사용할 수 있는 **프로세스 식별자(pid)**를 의미함
- **상호 배제(mutual exclusion)**가 없다면,  
  동일한 `pid`가 두 개의 서로 다른 프로세스에 **동시에 할당**될 수 있음!
![[Pasted image 20250519122246.png]]

# 운영체제에서의 임계 구역 처리 방식

## 운영체제에서 임계 구역을 처리하는 두 가지 일반적인 접근 방식

### 1. Non-preemptive (비선점형 커널)
- 커널 모드에서 실행 중인 프로세스는 **선점(preempt)**되지 않음
- 커널 모드 프로세스는 다음 중 하나가 될 때까지 계속 실행됨:
  - 커널 모드 종료
  - 블로킹 발생
  - 자발적으로 CPU 양도
- 결과적으로, 커널 모드에서는 **경쟁 조건이 거의 없음**

### 2. Preemptive kernels (선점형 커널)
- 커널 모드 실행 중인 프로세스도 **선점 가능**
- 더 빠르게 반응함:
  - 커널 모드 프로세스가 너무 오래 CPU를 점유할 위험이 줄어듦
- **실시간 프로세스(real-time process)**에 더 적합함


# 락(Locks): 기본 개념

## 목적
- **임계 구역(Critical Section)**이 마치 **단일 원자적 명령(atomic instruction)**처럼 실행되도록 보장함

## 예시: 공유 변수 업데이트
```c
balance = balance + 1;
```

## 임계 구역을 보호하는 코드 추가
```c
lock_t mutex;  // 전역으로 할당된 락 변수 ‘mutex’

lock(&mutex);
balance = balance + 1;
unlock(&mutex);
```
## 락 변수 (예: `mutex`)의 상태

- **Available (unlocked / free)**:  
    어떤 스레드도 락을 점유하지 않은 상태
- **Acquired (locked / held)**:  
    정확히 하나의 스레드가 락을 점유하고 있으며, 해당 스레드는 임계 구역에 있음


# 락(Locks): 기본 개념

## lock()
- 락을 획득하려고 시도함
- 다른 스레드가 락을 소유하고 있지 않다면, 현재 스레드가 락을 획득함
- 임계 구역에 진입
  - 이 스레드는 **락의 소유자(owner)**라고 불림
- 락을 보유한 스레드가 임계 구역에 있는 동안,  
  다른 스레드는 그 임계 구역에 진입할 수 없음

```c
lock(&mutex);
balance = balance + 1;
unlock(&mutex);
```
## unlock()
- 락의 소유자가 이 함수를 호출하면, 해당 락은 **다시 사용 가능(free)** 상태가 됨


# 락(Lock)의 요구 조건

## 1. 상호 배제 (Mutual Exclusion)
- 락이 **여러 스레드가 동시에 임계 구역에 들어가는 것을 방지**하는가?
- 임계 구역에 한 프로세스가 진입해 있으면, 다른 프로세스는 진입할 수 없어야 함

## 2. 공정성 (Fairness)
- 락을 얻으려는 **각 스레드가 공정하게 기회를 얻는가?**
- 관련 개념:
  - **기아(Starvation)**: 특정 스레드가 계속 락을 얻지 못하는 현상
  - **진행성(Progress)** 및 **유한 대기(Bounded Waiting)**

### 정의 (Operating System Concepts에서 발췌)
- **Mutual Exclusion**: 한 프로세스가 임계 구역을 실행 중이면, 다른 프로세스는 진입 불가
- **Progress**: 임계 구역을 실행 중인 프로세스가 없다면,  
  다음으로 진입할 프로세스를 결정하는 것이 **무기한 연기되어서는 안 됨**
- **Bounded Waiting**: 특정 프로세스가 락을 요청한 이후,  
  그 요청이 허용되기까지 **다른 프로세스들이 몇 번 임계 구역에 진입할 수 있는지 제한이 있어야 함**

## 3. 성능 (Performance)
- 락 사용으로 인해 추가되는 **시간 오버헤드**는 얼마나 되는가?

