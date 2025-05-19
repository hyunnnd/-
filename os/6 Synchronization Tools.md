
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



