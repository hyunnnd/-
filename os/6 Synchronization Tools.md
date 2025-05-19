
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


