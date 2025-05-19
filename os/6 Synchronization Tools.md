
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
