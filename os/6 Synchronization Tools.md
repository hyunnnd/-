
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

## 관련 개념

- **Race Condition**: 두 스레드 이상이 동시에 공유 자원에 접근하여 결과가 예측 불가능해지는 현상
- 해결 방안:
    - **동기화(synchronization)**: `mutex`, `semaphore`, `monitor` 등을 이용한 접근 제어