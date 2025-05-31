
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

Critical section problem은 프로세스가 데이터를 협력적으로 공유할 수 있도록 활동을 동기화하는 데 사용할 수 있는 프로토콜을 설계하는 것입니다
## 프로세스 흐름
- **Entry Section** → **Critical Section** → **Exit Section** → **Remainder Section**

> 공용 데이터 접근 시 반드시 임계 구역 보호 필요

## 각 구역 설명

- **Entry section**: 임계 구역 진입을 요청하는 코드 영역
- **Critical section**: 공유 자원(공용 변수, 파일 등)을 변경할 수 있는 코드 영역
- **Exit section**: 임계 구역에서 나왔음을 알리는 코드 영역
- **Remainder section**: 공유 자원을 변경하지 않는 나머지 코드 영역

![[Pasted image 20250526113718.png]]

# 운영체제에서의 임계 구역 처리 (Critical-Section Handling in OS)

## 예시: PID 할당 시 발생하는 경쟁 조건 (Race Condition)

- 프로세스 **P₀**와 **P₁**이 `fork()` 시스템 콜을 통해 자식 프로세스를 생성함
- 이때 커널 변수인 `next_available_pid`에 경쟁 조건이 발생함  
  → 이 변수는 다음에 사용할 수 있는 **프로세스 식별자(pid)**를 의미함
- **상호 배제(mutual exclusion)**가 없다면,  
  동일한 `pid`가 두 개의 서로 다른 프로세스에 **동시에 할당**될 수 있음!
![[Pasted image 20250519122246.png]]
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
- **임계 구역(Critical Section)이 마치 단일 원자적 명령(atomic instruction)**처럼 실행되도록 보장함

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

![[Pasted image 20250522121136.png]]

![[Pasted image 20250522121146.png]]

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

## 📌 Controlling Interrupts

### 🔒 Disable interrupts for critical sections

- 상호 배제를 제공하기 위한 가장 초기의 해결책 중 하나
- **단일 프로세서(single-processor)** 시스템을 위해 고안됨

`void lock() {     DisableInterrupts(); } void unlock() {     EnableInterrupts(); }`

### ⚠️ 문제점

- 애플리케이션에 대한 **신뢰**가 너무 많이 요구됨
    - 탐욕적(greedy) 또는 악의적인(malicious) 프로그램이 프로세서를 독점할 수 있음

- **다중 프로세서(multiprocessor)**에서는 작동하지 않음
    - 인터럽트를 마스킹하거나 해제하는 코드가 **현대 CPU에서 느리게 실행**됨

![[Pasted image 20250522121847.png]]


## 📌 Using a Flag

### ❗ 문제 1: **상호 배제 없음 (No Mutual Exclusion)**

> 초기 상태: `flag = 0`이라고 가정
#### Thread 1
`call lock() while (flag == 1)     // spin // context switch 발생`
#### Thread 2
`call lock() while (flag == 1) flag = 1;  // 동시에 설정됨! // context switch 발생`

결과:
`flag = 1;  // => 둘 다 진입 가능하게 되어 상호 배제가 깨짐`

### ❗ 문제 2: **Spin-waiting**

- 다른 스레드를 **기다리는 동안 CPU 시간 낭비**

### ✅ 해결책: **하드웨어에서 지원하는 원자적 명령이 필요함**

- `Test-and-Set` 명령 (atomic exchange라고도 함)


## 🔐 Test-And-Set

### ✅ 간단한 락(lock) 생성 지원을 위한 명령어

`int TestAndSet(int *ptr, int new) {     int old = *ptr;     // 현재 값을 가져옴     *ptr = new;         // 새로운 값을 저장     return old;         // 기존 값을 반환 }`

### 💡 핵심 개념

- `ptr`이 가리키는 **기존 값을 반환**함
- 해당 주소에 **새로운 값(new)을 설정**
- 이 일련의 연산은 **원자적으로 수행됨 (atomic)**


![[Pasted image 20250522122310.png]]



## 🔍 Evaluating Spin Locks

### ✅ **Correctness**: yes

- Spin lock은 한 번에 **하나의 스레드만 임계 구역에 진입**하도록 허용함

### ⚠ **Fairness**: no

- Spin lock은 **공정성(fairness)을 보장하지 않음**
- 어떤 스레드는 **무한히 대기(spin)**할 수도 있음

### ⚙ **Performance**

- 단일 CPU 환경에서는 **성능 오버헤드가 큼**
- **스레드 수가 CPU 수와 비슷한 경우**, Spin lock은 **상당히 잘 작동**함 (_reasonably well_)



## 🔁 Compare-And-Swap

### ✅ 동작 설명

- 주소 `ptr`이 가리키는 값이 `expected`와 같으면 → `ptr`에 `new` 값을 저장
- 그 외의 경우 → 변경 없이 **기존 값을 반환**


`int CompareAndSwap(int *ptr, int expected, int new) {     int actual = *ptr;     if (actual == expected)         *ptr = new;     return actual; }`

### 🔒 Spin Lock 예시 (CAS 기반)

`void lock(lock_t *lock) {     while (CompareAndSwap(&lock->flag, 0, 1) == 1)         ;  // spin }`

### 🧠 참고

- `cmpxchg` (compare-and-exchange): x86/x64에서 CAS 명령을 구현하는 어셈블리 명령어


## 🔄 Load-Linked and Store-Conditional (LL/SC)

### ✅ MIPS에서의 명령어 형식

- **Load-Linked (LL)**:  
    `ll rt, offset(rs)`
- **Store-Conditional (SC)**:  
    `sc rt, offset(rs)`
### 💻 구현 예시 (C 유사)

```c 
int LoadLinked(int *ptr) {     
	return *ptr; 
}  
int StoreConditional(int *ptr, int value) {     
	if (no one has updated *ptr since the LoadLinked to this address) {           
		*ptr = value;
	    return 1;  // success!     
	     } else {         
		return 0;  // failed to update    
	}
}
```

### ⚙ 동작 원리

- `StoreConditional()`은 **LL 이후 그 주소에 쓰기가 없었을 경우에만 성공**
    
    - 성공: `1` 반환 및 `*ptr = value`
    - 실패: `0` 반환 (값 변경 없음)


![[Pasted image 20250522124738.png]]



## ➕ Fetch-And-Add

### ✅ 개념

- **특정 주소의 값을 원자적으로 증가**시키고, 이전 값을 반환함

```c
int FetchAndAdd(int *ptr) {     
int old = *ptr;     
*ptr = old + 1;     
return old; 
}
```

### 🎟 Ticket Lock 구현 (with Fetch-And-Add)


```c 
typedef struct __lock_t {     
int ticket;     
int turn; 
} lock_t;  
void lock_init(lock_t *lock) {     
lock->ticket = 0;     
lock->turn = 0; 
}  
void lock(lock_t *lock) {     
int myturn = FetchAndAdd(&lock->ticket);     
while (lock->turn != myturn)         
		;  // spin 
}  
void unlock(lock_t *lock) {     
FetchAndAdd(&lock->turn); 
}
```
### ⚖ 장점

- 모든 스레드가 **순차적으로 진행 보장**  
    → **공정성(fairness)** 확보

# So Much Spinning

## 스핀락(Spin Lock)이란?
- **하드웨어 기반의 동기화 기법**으로, 락이 해제될 때까지 계속해서 루프를 돌며(lock 변수 검사) 대기하는 방식
- 구조는 **간단(simple)**하고 동작은 잘됨

## 문제점: 비효율성 (Inefficiency)
- **스핀 상태(Spinning)**에 빠진 스레드는 **계속해서 변수만 검사함**
- 그동안 **CPU 시간을 낭비**하며, 실질적인 작업은 수행하지 않음
- → **Time Slice(스케줄링 시간 조각)**가 낭비됨

> 예시:  
> 어떤 스레드가 락을 얻지 못해 계속 루프를 도는 동안, CPU는 아무 작업도 하지 않고 그 스레드의 루프만 수행하게 됨
## 결론
- 스핀락은 단순하고 빠를 수 있지만, **리소스가 한정된 환경** 또는 **다중 스레드 환경**에서는 **비효율적일 수 있음**


# A Simple Approach: Just Yield

- 스핀하려 할 때, **CPU를 다른 스레드에게 양보**함
  - OS 시스템 콜은 호출자를 **Running 상태에서 Ready 상태**로 이동시킴
- **Context switch** 비용이 상당할 수 있으며, **starvation** 문제도 여전히 존재함

## 코드 예시

```c
void init() {
    flag = 0;
}

void lock() {
    while (TestAndSet(&flag, 1) == 1)
        yield();  // give up the CPU
}

void unlock() {
    flag = 0;
}

int TestAndSet(int *ptr, int new) {
    int old = *ptr;
    *ptr = new;
    return old;
}
```


## 📌 제목: Using Queues and Sleeping

### ✅ 개요

- **Queue**를 사용하여 락을 기다리는 스레드들을 관리함
- **park()**: 호출한 스레드를 **수면 상태**로 전환
- **unpark(threadID)**: 특정 스레드를 **깨움**

✅ 자료구조 정의
```c
typedef struct __lock_t {
    int flag;        // lock is acquired or not
    int guard;       // to protect the queue
    queue_t *q;
} lock_t;
```

- `flag`: 락 보유 여부
- `guard`: 큐 보호용
- `q`: 대기 큐

✅ 초기화 함수
```c
void lock_init(lock_t *m) {
    m->flag = 0;
    m->guard = 0;
    queue_init(m->q);
}
```
![[Pasted image 20250526115956.png]]
### 🔑 요점 정리

- 락을 얻지 못한 스레드는 **큐에 저장되고 park() 호출로 대기 상태**가 됨
- 락 해제 시, **큐에서 다음 스레드를 꺼내 unpark()로 깨움**
- `guard`는 큐 접근 동기화를 위한 **스핀락**으로 사용됨


## 📌 개념: Wakeup/Waiting Race

### 문제 상황

- **Thread A**가 락을 **해제하는 순간**, **Thread B**가 `park()`를 **막 호출하려는 타이밍**이라면,
- **Thread A가 `unpark()`를 먼저 호출하고**, **Thread B는 그 후에 `park()`를 호출**하게 될 수 있음
- 이 경우, **Thread B는 영원히 잠들 수 있음 (potential sleep forever)**  
	→ `unpark()`는 이미 호출됐고, `park()`는 이제 막 호출되어 블로킹됨

## 🛠 Solaris의 해결 방법: `setpark()`

### 핵심 아이디어

- 새로운 시스템 콜 `setpark()`를 사용하여,  
    **스레드가 곧 `park()`를 호출할 것임을 미리 알림**
### 동작 원리

1. `setpark()`를 먼저 호출하면, **"이제 park할 준비됨"** 이라는 **상태 비트**가 설정됨
2. 이후 다른 스레드가 `unpark()`를 먼저 호출하더라도,  
    `park()` 호출 시 즉시 리턴되어 **잠들지 않음**
3. 이렇게 하면 **경쟁 조건(race condition)**이 제거됨

## 🔧 코드 수정 예시 (락 획득 함수 내부)

```c
// Code modification inside of lock()
queue_add(m->q, gettid());
setpark();          // new code: tell OS "about to park"
m->guard = 0;
park();             // if unpark() already called, returns immediately
```
- `setpark()`는 `park()` 전에 호출되어야 하며,
- `unpark()`가 먼저 호출되더라도 스레드는 블로킹되지 않음

## ✅ 요약

| 항목  | 설명                                         |
| --- | ------------------------------------------ |
| 문제  | `unpark()`가 먼저 호출되면 `park()`는 영원히 블록될 수 있음 |
| 해결  | `setpark()`를 통해 사전 알림 → OS가 상태 기억          |
| 효과  | `park()`는 필요 없을 경우 **즉시 리턴**하여 블로킹 없음      |

## 📌 Futex (Fast Userspace Mutex)

### ✅ 개요

- **Linux**에서 제공하는 동기화 메커니즘
- **Solaris의 `park()` / `unpark()`와 유사한 기능**
- 유저 공간에서 빠르게 동작하도록 설계됨

## ✅ 핵심 특징

- **futex()** 시스템 콜은 **특정 조건이 만족될 때까지 대기**할 수 있도록 해줌    
- 유저 공간에서 조건을 먼저 검사하고, 필요할 때만 커널에 진입함 → 효율적임

## ✅ 주요 함수

### `futex_wait(address, expected)`

- 호출한 스레드를 **수면 상태**로 전환
- `address`에 저장된 값이 `expected`와 다르면, **즉시 반환됨**
    - 즉, 대기할 필요가 없다고 판단됨
### `futex_wake(address)`

- 해당 `address`에 대해 대기 중인 **스레드 하나를 깨움**

## 🔁 동작 요약

|상황|동작|
|---|---|
|조건이 충족되지 않음|유저 공간에서 대기 없이 빠르게 반환|
|조건이 충족될 때까지 대기|커널로 진입하여 **수면 → 웨이크업**|

## ✅ 유용한 점

- **Busy-waiting을 피하면서도 성능을 최대화**할 수 있음
- 고성능 사용자 공간 락 구현에 많이 사용됨 (예: pthread, glibc 내부)


## 📌 **Futex – `lowlevellock.h` in the `nptl` library**

### ✅ 핵심 개념

- `futex`를 활용한 사용자 공간 기반의 **경량 락(mutex)** 구현 예시
- **정수형 변수** `*mutex`의 비트를 다음과 같이 활용함:
    - **상위 비트 (Bit 31)**: 락이 **보유 중인지 여부**
    - **하위 비트들**: 해당 락을 **기다리는 스레드 수**
![[Pasted image 20250526120557.png]]
### 📌 요약

- 처음에는 **빠른 경로(fast path)**를 시도: Bit 31이 0인지 확인
- 실패하면 **대기자 수 증가**, 루프 안에서 계속 확인
- 조건이 충족되지 않으면 `futex_wait()` 호출로 **슬립 진입**
- 락을 해제하며 0x80000000을 더함 → Bit 31 해제
- 다른 스레드가 대기 중이라면 **futex_wake()로 하나 깨움**


## 📌 Two-Phase Locks

### ✅ 개념

- **락이 곧 해제될 가능성이 있을 때**, **스핀**을 잠깐 시도해 보는 것이 효과적일 수 있음
- 이를 반영한 **락 획득 전략**이 **Two-Phase Lock (2단계 락)**

## 🔄 단계 설명

### 1. **First Phase: Spinning**

- 일정 시간 동안 **락을 얻기 위해 계속 시도(스핀)**
- 락을 곧 얻을 수 있다고 기대할 때 효과적
- 단, 이 시도 중에 락을 얻지 못하면 **두 번째 단계로 진입**
### 2. **Second Phase: Sleeping**

- 일정 시간 동안 락을 얻지 못하면 **스레드를 슬립 상태로 전환**
- 이후 락이 해제되었을 때 **다른 스레드가 깨움**


# 조건 변수 (Condition Variables)

- 스레드가 실행을 계속하기 전에 조건이 참인지 확인하고자 하는 경우가 자주 있음

- 예시:  
  - 부모 스레드는 자식 스레드가 완료되었는지 확인하고자 할 수 있음  
  - 이는 흔히 `join()`이라고 불림


![[Pasted image 20250526121642.png]]


# 조건을 기다리는 방법 (How to Wait for a Condition)

## 조건 변수 (Condition variable)
### **스레드들의 큐**를 관리함
### 조건에 대해 대기 (Waiting on the condition)
- 실행 상태가 원하는 상태가 아닐 경우, 스레드가 **명시적으로 큐에 자신을 등록**함
### 조건에 대해 신호 보내기 (Signaling on the condition)
- 다른 스레드가 **상태를 변경하면**, 대기 중인 스레드 중 **하나를 깨워서** 계속 실행하도록 함
## 하나의 패키지에 포함되는 세 가지 요소 (Three in a package)
- 조건 변수 `c`
- 상태 변수 `m`
- 상태 변수를 보호하기 위한 `lock L`


# POSIX 조건 변수 (POSIX Condition Variables)

- POSIX 조건 변수는 상호 배제를 제공하기 위해 POSIX 뮤텍스 락과 함께 사용됨

## 생성 및 초기화

```c
pthread_mutex_t m;
pthread_cond_t c;

pthread_mutex_init(&m, NULL);
pthread_cond_init(&c, NULL);
```


# POSIX 조건 변수 - 동작 방식

## 조건 a == b 가 될 때까지 대기하는 스레드

```c
pthread_mutex_lock(&m);
while (a != b)
    pthread_cond_wait(&c, &m);
pthread_mutex_unlock(&m);
```
`wait()` 호출은 뮤텍스를 인자로 받음

- `wait()`는 락을 **해제하고**, 호출한 스레드를 **수면 상태**로 만듦
- 스레드가 다시 깨어나면, **락을 다시 획득해야 함**

## 조건 변수를 통해 다른 스레드에게 신호를 보내는 스레드
```c
pthread_mutex_lock(&m);
a = b;
pthread_cond_signal(&c);
pthread_mutex_unlock(&m);
```

![[Pasted image 20250526122321.png]]


# 자식 스레드를 기다리기: 조건 변수

## 부모 (Parent)
- 자식 스레드를 생성하고 본인의 작업을 계속 수행함
- 자식 스레드가 종료될 때까지 기다리기 위해 `thr_join()`을 호출함
  - 락을 획득함
  - 자식 스레드가 완료되었는지 확인함
  - 완료되지 않았으면 `wait()` 호출로 자신을 수면 상태로 전환함
  - 락을 해제함

## 자식 (Child)
- "child"라는 메시지를 출력함
- `thr_exit()`를 호출하여 부모 스레드를 깨움
  - 락을 획득함
  - 상태 변수 `done`을 설정함
  - 부모에게 신호를 보내서(parent) 깨어나도록 함


# 상태 변수 `done`의 중요성

## 코드 예시

```c
void thr_exit() {
    pthread_mutex_lock(&m);
    pthread_cond_signal(&c);
    pthread_mutex_unlock(&m);
}

void thr_join() {
    pthread_mutex_lock(&m);
    pthread_cond_wait(&c, &m);
    pthread_mutex_unlock(&m);
}
```
## 문제 상황 (자식이 먼저 실행되는 경우)

- 자식 스레드가 먼저 실행되면 `signal()`을 호출하지만,  
    그 시점에는 **조건 변수에서 자고 있는 스레드가 없음**
- 이후 부모 스레드가 실행되면서 `wait()`를 호출하게 되면,  
    **이미 신호는 지나간 상태라 깨어날 수 없음**
- 결과적으로 부모 스레드는 `wait()`에서 **영원히 대기 상태에 빠짐**

> 🧠 따라서, 이 문제를 방지하려면 `done`과 같은 **상태 변수**를 활용해  
> 조건이 이미 충족되었는지를 먼저 확인해야 함

# 또 다른 잘못된 구현 (Another Poor Implementation)

## 코드 예시

```c
void thr_exit() {
    done = 1;
    pthread_cond_signal(&c);
}

void thr_join() {
    if (done == 0)
        pthread_cond_wait(&c);
}
```
## 문제: 미묘한 경쟁 조건 (Subtle Race Condition)

- **부모 스레드가 `thr_join()`을 호출함**
    - `done` 값을 확인 → `0`이면 대기하려고 함
    - 대기(`wait`)를 호출하기 직전에 **스레드가 중단**되고, 그 사이에 **자식 스레드가 실행됨**

- **자식 스레드가 `done = 1`로 설정하고 signal을 보냄**
    - 이때는 **대기 중인 스레드가 없음** → 신호가 사라짐

- 부모 스레드가 다시 실행되면    
    - `wait()`를 호출하고 **영원히 수면 상태에 빠짐**
    - 아무도 다시 깨워주지 않음
## 결론

- 상태 검사(`done == 0`)와 `wait()` 호출 사이에 **락이 없기 때문에**  **경쟁 조건**이 발생함
- 이를 방지하려면 반드시 **락을 사용하여 상태 변수 검사와 대기 동작을 원자적으로 보호**해야 함


# 유한 버퍼 (Bounded Buffer)

- 유한 버퍼는 한 프로그램의 출력을 다른 프로그램에 파이프로 전달할 때 사용됨

## 예시: `grep foo file.txt | wc -l`
- `grep` 프로세스는 **생산자 (producer)**
- `wc` 프로세스는 **소비자 (consumer)**
- 이 둘 사이에는 커널 내부의 **유한 버퍼**가 존재함

- 유한 버퍼는 **공유 자원**이므로  → **동기화된 접근**이 반드시 필요함!


![[Pasted image 20250526123721.png]]


# 생산자/소비자 문제: 단일 조건 변수와 if 문 사용

## 설정
- 하나의 조건 변수 `cond`와 뮤텍스 `mutex`를 사용함

## 생산자 (Producer)

```c
void *producer(void *arg) {
    int i;
    for (i = 0; i < loops; i++) {
        pthread_mutex_lock(&mutex);       // p1
        if (count == 1)                   // p2
            pthread_cond_wait(&cond, &mutex); // p3
        put(i);                           // p4
        pthread_cond_signal(&cond);       // p5
        pthread_mutex_unlock(&mutex);     // p6
    }
}
```

## 소비자 (Consumer)
```c
void *consumer(void *arg) {
    int i;
    for (i = 0; i < loops; i++) {
        pthread_mutex_lock(&mutex);       // c1
        if (count == 0)                   // c2
            pthread_cond_wait(&cond, &mutex); // c3
        int tmp = get();                  // c4
        pthread_cond_signal(&cond);       // c5
        pthread_mutex_unlock(&mutex);     // c6
        printf("%d\n", tmp);
    }
}
```
## 설명

- **p1–p3**: 생산자는 버퍼가 **비어 있을 때까지 기다림**
- **c1–c3**: 소비자는 버퍼가 **가득 찰 때까지 기다림**
## 조건

- 단일 생산자와 단일 소비자일 경우 위 코드로 충분함

> ⚠ 다중 생산자/소비자 환경에서는 이 구현은 문제가 발생할 수 있음

![[Pasted image 20250531213810.png]]


## Thread Trace: Broken Solution (Version 1)

### ● 문제 발생 원인
- 프로듀서가 소비자 스레드 `Tc1`을 깨운 이후, `Tc1`이 실제로 실행되기 전에 **다른 소비자 스레드 `Tc2`가 버퍼 상태를 변경**함.
- 따라서, 깨어난 `Tc1`이 실행될 때 **버퍼 상태가 여전히 유효하다는 보장이 없음**.
- 이는 **Mesa Semantics**에 해당함.

### ● Mesa Semantics
- 깨어난 스레드가 즉시 실행된다는 보장이 없음.
- 대부분의 실제 운영체제는 Mesa Semantics를 사용함.

### ● Hoare Semantics
- 깨어난 스레드가 **즉시 실행됨**을 보장함.
- 더 강력한 의미적 보증을 제공하지만, 실제 구현은 거의 없음.


