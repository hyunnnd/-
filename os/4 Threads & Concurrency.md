## 📘 Overview

- **프로세스(Process)**: 실행 중인 프로그램
- **스레드(Thread)**: 프로그램이 자신을 여러 작업으로 나누어 동시에 실행할 수 있는 단위
- 스레드는 프로세스보다 작은 단위
- 하나의 프로세스 내 스레드들은 자원을 공유
- 스레드는 다음 정보를 가짐:
  - Thread ID, Program Counter, Register Set, Stack 등


![[Pasted image 20250513182928.png]]

## 📘 Address Spaces

- 각 스레드는 **자기만의 스택(Stack)**을 가짐
- 그 외 자원(코드, 힙 등)은 **공유됨**

### 🔹 메모리 구조:
- **코드(Code)**: 프로그램 명령어
- **힙(Heap)**: 동적 할당 데이터 (malloc 등)
- **스택(Stack)**: 지역 변수, 함수 호출 인자 등
- ![[Pasted image 20250513182957.png]]

> 단일 스레드 vs. 두 개의 스레드가 있는 메모리 구조를 시각적으로 비교함



## 📘 Thread Control Block

- **TCB (Thread Control Block)**: 스레드 관리에 필요한 정보를 담은 커널의 자료구조
- Linux에서는 `thread_struct`

### 🔹 포함 정보:
- Thread ID
- 현재 상태 (running, ready, waiting 등)
- 스택 포인터, 프로그램 카운터
- 레지스터 값
- 해당 스레드가 속한 **PCB (Process Control Block)**의 포인터


## 📘 Why Use Threads?

- **프로세스 생성은 시간과 자원 측면에서 비용이 큼**
- 예: 웹 서버는 수천 개의 요청을 처리해야 하므로, 프로세스보다 스레드가 효율적
![[Pasted image 20250513183245.png]]
> 병렬 처리와 효율적 자원 활용을 위한 수단으로 스레드 사용

- **병렬성(Parallelism)** 확보 가능:
  - 단일 스레드 프로그램은 직관적이지만 느림
  - 멀티스레드 프로그램은 여러 CPU를 활용하여 속도 향상

- **I/O 지연으로 인한 블로킹 방지**:
  - 스레드는 I/O 작업과 다른 연산을 **동시에 처리** 가능
  - 이는 프로세스 기반 멀티프로그래밍과 유사하지만, **프로세스 간보다 비용 적음**
### 🔹 스레드 사용의 장점:
- **응답성 (Responsiveness)**: 일부가 블록돼도 나머지 작업은 계속 수행 가능 → UI에 중요
- **자원 공유 (Resource Sharing)**: 동일 프로세스 내 자원 공유가 쉬움
- **경제성 (Economy)**: 프로세스보다 생성·전환 비용이 저렴
- **확장성 (Scalability)**: 다중 프로세서를 효과적으로 활용 가능

## 📘 Parallelism and Concurrency

### 🔹 멀티코어/멀티프로세서 환경에서의 도전 과제:
- 작업 분할
- 부하 균형
- 데이터 분할
- 데이터 의존성
- 테스트 및 디버깅

### 🔹 개념 구분:
- **Parallelism**: 여러 작업을 **실제로 동시에 수행**
- **Concurrency**: 여러 작업이 **동시에 진행되는 것처럼 보이도록 구성**
  - 단일 코어에서도 **스케줄링**을 통해 가능


## 📘 Concurrency vs. Parallelism

- **Concurrency**: 단일 코어에서도 시간 분할로 여러 작업이 **동시에 실행되는 듯이 보임**
- **Parallelism**: 멀티코어 시스템에서 **물리적으로 동시에 실행**
![[Pasted image 20250513185654.png]]

> ⬅ 슬라이드에는 두 가지를 시각적으로 비교한 도식 포함


## 📘 Types of Parallelism

- **데이터 병렬성 (Data Parallelism)**:
  - 동일한 작업을 서로 다른 데이터 부분에 대해 수행
  - ex) 벡터 요소를 병렬로 처리

- **작업 병렬성 (Task Parallelism)**:
  - 각 스레드가 **서로 다른 작업**을 수행
  - ex) 한 스레드는 렌더링, 다른 스레드는 입출력 처리
![[Pasted image 20250513185728.png]]


## 📘 Amdahl’s Law

- 병렬 처리에서 성능 향상 한계를 제시하는 법칙
- 직렬 및 병렬 구성 요소를 모두 갖춘 애플리케이션에 코어를 추가하여 성능 향상을 식별합니다

### 🔹 식:
![[Pasted image 20250513185816.png]]
- S: 프로그램의 직렬 부분 비율
- N: 프로세서 수

### 🔹 예:
- 프로그램의 75%가 병렬 가능할 때, 2개 코어 사용 → **1.6배 속도 향상**
- N이 무한대로 가도, 속도 향상 한계는 `1/S`

> 직렬 구간이 클수록 병렬화의 이점이 제한됨


## 📘 User Threads and Kernel Threads

### 🔹 User Thread (사용자 스레드)
- 사용자 공간의 스레드 라이브러리에 의해 관리됨
- **시스템 콜 없이 생성** 가능 (ex. `pthread_create`)
- 커널은 존재를 모름 → **스케줄링 빠름**
- 전환 비용이 작지만, **한 스레드의 블록이 전체 스레드 블록 유발 가능**

### 🔹 Kernel Thread (커널 스레드)
- 커널에 의해 생성 및 관리
- 커널 수준에서 직접 스케줄링
- **사용자 스레드보다 전환 비용이 큼**, 하지만 병렬 실행 가능

> 대부분의 OS는 커널 스레드 기반 멀티스레딩 지원


## 📘 Kernel Thread

- 운영체제 커널 자체도 보통 **멀티스레드 구조**
- 커널 스레드는 다양한 작업을 병렬로 수행
  - ex) 디바이스 관리, 메모리 관리, 인터럽트 처리 등

> 커널 스레드는 시스템 전반의 병렬성과 응답성을 높이는 데 핵심 역할 수행


## 📘 Multithreading Models 개요

- **주요 이슈**: 사용자 스레드 ↔ 커널 스레드 간 **매핑 방식**

### 🔹 세 가지 모델:
1. **Many-to-One**
2. **One-to-One**
3. **Many-to-Many (또는 Two-Level)**

> 각 모델은 사용자 스레드와 커널 스레드의 연결 구조 및 효율성에서 차이 발생


## 📘 Many-to-One

- 여러 **사용자 스레드**가 하나의 **커널 스레드**에 매핑됨
- 한 스레드가 블록되면 → **전체 사용자 스레드 블록**
- **멀티코어 환경에서 병렬 실행 불가**

### 🔹 단점:
- 병렬성 없음
- 시스템 전체가 한 스레드로 인식됨

### 🔹 사용 예:
- Solaris Green Threads (과거)
- 초기 Java 스레드 라이브러리

## 📘 One-to-One

- 각 사용자 스레드가 **하나의 커널 스레드에 매핑**
- **병렬성 확보 가능**
- 사용자 스레드를 만들 때 커널 스레드도 생성됨 → **오버헤드 존재**

### 🔹 장점:
- 각 스레드가 독립적으로 스케줄링 가능
- 병렬성 지원

### 🔹 단점:
- 커널 리소스 소모 ↑ → **스레드 수 제한 가능성**

### 🔹 사용 예:
- Windows, Linux

## 📘 Many-to-Many

- 여러 사용자 스레드 ↔ 여러 커널 스레드로 **유연하게 매핑 가능**
- OS가 커널 스레드를 적절히 생성/관리

### 🔹 장점:
- 병렬성 확보 + 사용자 스레드 수 제한 없음
- 효율적 자원 사용 가능

### 🔹 예시:
- Windows의 ThreadFiber 패키지
- 그 외에는 드물게 사용

> 두 가지 방식의 절충형 → **Two-Level Model**로도 불림



## 📘 Thread Libraries

- **스레드 라이브러리**는 스레드 생성을 위한 **API를 제공**

### 🔹 구현 방식:
1. **사용자 수준(User-level)**:
   - 커널 개입 없이 사용자 공간에서만 관리
   - 전환 빠름, 하지만 커널은 스레드 존재를 인식하지 못함

2. **커널 수준(Kernel-level)**:
   - 스레드 생성/관리 모두 커널이 수행
   - 병렬 실행 가능, 시스템 호출 필요

> 라이브러리는 프로그래머가 직접 스레드 생성·관리하지 않고도 멀티스레딩 구현 가능



## 📘 Pthreads

- **Pthreads (POSIX Threads)**: POSIX 표준 (IEEE 1003.1c)에 따른 스레드 API
- UNIX 계열 운영체제(Solaris, Linux, macOS 등)에서 널리 사용

### 🔹 특징:
- Pthreads는 **명세(specification)**이며, 구현은 시스템에 따라 다름 not implementation
- 사용자 수준 또는 커널 수준 모두 구현 가능

### 🔹 제공 기능:
- 스레드 생성 (`pthread_create`)
- 동기화 (`pthread_join`, mutex 등)
- 종료, 취소 등 다양한 API

> POSIX 표준에 따라 다양한 시스템에서 **이식성 높은 멀티스레딩** 구현 가능



## 📘 pthread_create

- **pthread_create()**: 새로운 스레드를 생성하는 함수 
```c
#include <pthread.h>
int pthread_create(pthread_t *thread, const pthread_attr_t *attr,
                   void *(*start_routine) (void *), void *arg);
                   
```
### 🔹 매개변수:

- `thread`: 생성된 스레드의 ID를 저장할 변수
- `attr`: 스레드 속성 설정 (NULL 시 기본 속성 사용)
- `start_routine`: 스레드가 실행할 함수
- `arg`: 스레드 함수로 전달할 인자

### 🔹 반환값:

- 성공: 0
- 실패: 오류 코드

> 스레드는 **start_routine 함수에서 시작하여 종료**됨


## 📘 pthread_join 

- **pthread_join()**: 특정 스레드가 종료될 때까지 호출한 스레드를 **대기 상태로 만듦** , 이미  종료됬다면 즉시 실행(맞는지 체크해보기)
```c
#include <pthread.h>
int pthread_join(pthread_t thread, void **retval);
```

### 🔹 매개변수:

- `thread`: 대기할 스레드의 ID
- `retval`: 종료된 스레드가 반환한 값 (필요 없으면 NULL)

### 🔹 특징:

- **차단 함수**: 대상 스레드가 끝날 때까지 호출자 스레드는 실행되지 않음
- 부모 스레드가 종료되면 **자식 스레드도 강제 종료됨**

> `pthread_join`은 스레드 간의 **종속성 제어** 및 **자원 회수**에 필수적



## 📘 Compiling and Running

### 🔹 컴파일 방법:
- 반드시 `pthread.h` 헤더 포함
- **`-pthread` 옵션 추가 필요**

```bash
$ gcc -o main main.c -Wall -pthread
```
>최신 gcc는 `-pthread` 옵션을 자동으로 포함하기도 함



## 📘 Example – Thread Creation

```c
#include <stdio.h>
#include <pthread.h>

void *mythread(void *arg) {
    printf("%s\n", (char *) arg);
    return NULL;
}

int main() {
    pthread_t p1, p2;

    printf("main: begin\n");
    pthread_create(&p1, NULL, mythread, "A");
    pthread_create(&p2, NULL, mythread, "B");

    pthread_join(p1, NULL);
    pthread_join(p2, NULL);
    printf("main: end\n");
    return 0;
    }
```

### 🔹 설명:

- `mythread` 함수는 인자로 받은 문자열을 출력
- `main` 함수에서 두 개의 스레드를 생성하고 종료될 때까지 대기 (`join`)
- 실행 결과는 A, B의 출력 순서가 **비결정적** (스케줄러에 따라 달라짐)

![[Pasted image 20250515114931.png]]


## 📘 Example – Passing Argument to Thread

```c
#include <stdio.h>
#include <pthread.h>

int sum = 0;

void *thread_summation(void *arg) {
    int start = ((int*)arg)[0];
    int end = ((int*)arg)[1];
    while (start <= end) {
        sum += start;
        start++;
    }
    return NULL;
}

int main() {
    pthread_t id_t1, id_t2;
    int range1[] = {1, 5};
    int range2[] = {6, 10};

    pthread_create(&id_t1, NULL, thread_summation, (void *)range1);
    pthread_create(&id_t2, NULL, thread_summation, (void *)range2);

    pthread_join(id_t1, NULL);
    pthread_join(id_t2, NULL);

    printf("result: %d\n", sum);
    return 0;
} 
```
🔹 핵심:
배열을 인자로 전달하여 범위 설정
전역 변수 sum에 누적 합 저장
결과: 1 + 2 + ... + 10 = 55


## 📘 Example – Return Value from Thread

```c
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>

typedef struct {
    int a;
    int b;
} myarg_t;

typedef struct {
    int x;
    int y;
} myret_t;

void *mythread(void *arg) {
    myarg_t *m = (myarg_t *) arg;
    printf("%d %d\n", m->a, m->b);

    myret_t *r = malloc(sizeof(myret_t));
    r->x = 1;
    r->y = 2;
    return (void *) r;
}

int main() {
    pthread_t p;
    myret_t *m;
    myarg_t args = {10, 20};

    pthread_create(&p, NULL, mythread, &args);
    pthread_join(p, (void **) &m);
    printf("returned %d %d\n", m->x, m->y);
    return 0;
}
```
🔹 핵심:
구조체 인자를 전달하고, 동적 할당된 구조체를 리턴 받아 사용
결과: 10 20 
	returned 1 2
주의: malloc 없이 스택 변수 반환 시 위험 (다음 슬라이드에서 설명)


## 📘 Dangerous Code

- 스레드에서 **반환값으로 지역 변수(스택 메모리)**를 리턴하면 위험

```c
void *mythread(void *arg) {
    myarg_t *m = (myarg_t *) arg;
    printf("%d %d\n", m->a, m->b);

    myret_t r;  // ❌ 스택에 할당된 변수
    r.x = 1;
    r.y = 2;
    return (void *) &r;  // ⛔ 위험: 함수 종료 시 메모리 해제됨
}
```
### 🔹 해결책:

- 반환할 데이터는 **힙(heap)** 에 `malloc`으로 동적 할당하여 반환해야 함


## 📘 Multiple Threads

- **여러 개의 스레드를 생성하고 모두 조인하는 예제**

```c
#define NUM_THREADS 10
pthread_t workers[NUM_THREADS];

for (int i = 0; i < NUM_THREADS; i++)
    pthread_create(&workers[i], NULL, thread_worker, (void *)arg);

for (int i = 0; i < NUM_THREADS; i++)
    pthread_join(workers[i], NULL);
```
🔹 핵심:
스레드 ID를 배열로 관리
반복문으로 스레드 생성 및 정리


## 📘 Pthread Cancel

- **pthread_cancel()**: 실행 중인 스레드에 취소 요청을 보냄

```c
#include <pthread.h>
int pthread_cancel(pthread_t thread);
```
🔹 특징:
	thread: 취소할 대상 스레드 ID
	실제로 즉시 종료되지 않을 수 있음 (해당 스레드의 상태와 속성에 따라 다름)
	취소 동작은 cancelability state와 cancelability type에 의해 제어됨
	안전한 취소를 위해 스레드 측에서도 적절한 설정 필요


## 📘 Example – Pthread Cancel

- 두 개의 스레드가 있으며, **counter 값이 5가 되면 다른 스레드를 종료시킴**

```c
void* func1(void* arg) {
    while (1) {
        printf("Thread #1 (counter=%d)\n", counter);
        if (counter == 5) {
            pthread_cancel(tmp_thread);  // 다른 스레드 취소 요청
            pthread_exit(NULL);          // 자신 종료
        }
        sleep(1);
    }
}

void* func2(void* arg) {
    tmp_thread = pthread_self();  // 자신의 스레드 ID 저장
    while (1) {
        printf("Thread #2 (counter=%d)\n", counter);
        counter++;
        sleep(1);
    }
}

int main() {
pthread_tthread_one, thread_two; 
// create thread_oneand thread_two
pthread_create(&thread_one, NULL, func1, NULL);
pthread_create(&thread_two, NULL, func2, NULL); 

// waiting for when threads are completed
pthread_join(thread_one, NULL); 
pthread_join(thread_two, NULL); 
return 0;
}
```
![[Pasted image 20250515121825.png]]
func1이 func2를 종료시키고 본인은 정상 종료함


## 📘 Thread Cancellation

- 스레드는 **자원을 공유**하므로 취소 시 **데이터 무결성**에 주의해야 함  
  (프로세스는 자원을 독립적으로 가짐)

### 🔹 취소 관련 기본 설정

```c
int pthread_setcanceltype(int type, int *oldtype);
```
`type`:
- `PTHREAD_CANCEL_ASYNCHRONOUS`: 즉시 종료
- `PTHREAD_CANCEL_DEFERRED`: 특정 지점에서만 종료 (기본값)
- 비동기보다 지연 취소가 **안전성 측면에서 선호됨**

```c
int pthread_setcancelstate(int state, int *oldstate);
```
`state`:
- `PTHREAD_CANCEL_DISABLE`: 취소 요청은 보류 
- `PTHREAD_CANCEL_ENABLE`: 취소 요청 수락
- `oldstate`: 이전 상태 저장용 포인터 (필요 없다면 `NULL` 가능)

🔹 Deferred Cancellation의 특징
- 취소는 스레드 내부에서 **취소 지점**(`pthread_testcancel()`)에 도달해야 발생
- 취소 발생 시 **clean-up handler**가 호출됨    
- **스레드 안전성 확보를 위한 중요한 기법**
### 🔹 요약

|타입|설명|안전성|
|---|---|---|
|`PTHREAD_CANCEL_ASYNCHRONOUS`|즉시 종료|낮음 (위험)|
|`PTHREAD_CANCEL_DEFERRED`|취소 지점에서만 종료 (기본값)|높음 (권장)|

### 취소의 처리 시점

- **지연 취소(Deferred cancellation)** 방식에서는,
    - 스레드가 명시적으로 **취소 지점**(`pthread_testcancel()`)에 도달해야 취소됨
    - 취소 요청은 **대기(pending)** 상태로 남음
- **비동기 취소(Asynchronous cancellation)**는 요청 즉시 취소됨 → **자원 해제 미보장 위험**

## 📘 예제 – Thread Cancellation

### 🔹 코드 요약

```c
pthread_setcanceltype(*(int*)arg, NULL);  // 스레드 내에서 취소 타입 설정
```
- 취소 방식(비동기 / 지연)은 `arg`로 전달
- `counter`를 40억까지 증가시키는 반복 루프 실행

```c
pthread_cancel(thread);  // 3초 뒤 스레드 취소 요청 pthread_join(thread, NULL);  // 스레드 종료 대기
```
### 🔹 주요 실행 흐름

1. 첫 번째 스레드: **비동기 취소 방식**
    - 3초 후 `pthread_cancel()` → 즉시 종료
    - 종료 시점의 `counter` 출력
2. 두 번째 스레드: **지연 취소 방식**
    - `pthread_testcancel()`이 주석 처리되어 있어 → 취소 요청에 **응답하지 않음**
    - 따라서 종료되지 않음
![[Pasted image 20250515123626.png]]
> ❗ `pthread_testcancel()`을 명시적으로 호출해야 지연 취소 방식에서 정상 종료 가능
### 🔹 결과 요약

| 방식               | 반응 시점      | 특징               |
| ---------------- | ---------- | ---------------- |
| 비동기 취소 (async)   | 즉시         | 불안정, 자원 누수 위험 있음 |
| 지연 취소 (deferred) | 취소 지점 도달 시 | 안정적, 명시적 취소 필요   |


## 📘 Thread-Local Storage

- 하나의 **프로세스 내 모든 스레드는 전역 변수**를 공유함
- **Thread-local storage (TLS)**는 **각 스레드마다 고유한 데이터 복사본**을 가짐

```c
__thread int tls;  // pthread에서 사용
```
- 각 스레드는 독립된 `int tls` 변수를 가짐
- **일반 지역 변수와 다름**: 지역 변수는 함수 호출 중에만 존재
- **TLS는 함수 호출 간에도 유지되며**, 정적 변수와 유사함
- TLS는 **각 스레드에 대해 고유하게 유지**됨
- 스레드 생성 과정을 직접 제어할 수 없는 경우(예: **스레드 풀**)에 유용함


## 📘 Thread-Local Storage in pthread

```c
`#define THREADS 3 __thread int tls; int global;
```

- `__thread`로 선언된 `tls`: **스레드마다 독립적인 값** 유지
- 일반 전역 변수 `global`: **모든 스레드가 공유**

```c
void *func(void *arg) {     int num = *((int*)arg);     tls = num;     global = num;     sleep(1);     printf("Thread = %d tls = %d global = %d\n", num, tls, global); }
```

- 각 스레드는 `tls`에 고유한 값을 저장
- 출력 시 `tls`는 예상한 값 유지, `global`은 마지막 스레드 값으로 덮임


## 📘fork() and exec()

### 🔹 fork() 호출 시

- **멀티스레드 프로세스에서 fork()** 호출 시 동작 방식:
    
    - 모든 스레드를 복제하는가?
    - 호출한 **단일 스레드만 복제**하는가?

→ **UNIX는 두 가지 버전 제공**:

- `fork()`: 전체 복제
- `fork1()`: 하나의 스레드만 복제

### 🔹 exec() 호출 시

- **멀티스레드 프로세스에서 exec()** 호출 시:
    - 프로세스 전체가 **새로운 프로그램으로 완전히 대체됨**
    - 모든 기존 스레드는 제거됨


## 📘 Linux Threads

### 🔹 Linux에서의 스레드

- Linux는 **스레드를 "태스크(task)"**로 표현함
- **`clone()` 시스템 호출**을 통해 스레드 생성

### 🔹 clone()의 특징

- `clone()`은 자식 태스크가 부모의 **주소 공간을 공유**하도록 허용
- **플래그(flags)**를 통해 어떤 자원을 공유할지 지정 가능

### 🔹 내부 구조

- 각 태스크는 `struct task_struct`를 통해 공유 또는 독립적인 **프로세스 관련 데이터 구조**를 참조



# Implicit Threading

## 정의
- **Implicit Threading**이란:  
  스레드의 생성과 관리를 **프로그래머가 아닌** 컴파일러와 런타임 라이브러리가 수행하는 방식입니다.

## 주요 기법
1. **Thread Pools**  
2. **Fork-Join Model**  
3. **OpenMP**

## 그 외 방법
- **Grand Central Dispatch (GCD)** – Apple 시스템에서 사용  
- **Microsoft Threading Building Blocks (TBB)** – Intel에서 제공  
- 기타 다양한 라이브러리 존재


# Thread Pools

## 개요
- 여러 개의 스레드를 **풀(Pool)** 형태로 생성하여, 작업이 주어질 때까지 대기시킴

## 장점
- 기존 스레드를 재사용하여 새로운 스레드를 생성하는 것보다 **요청 처리 속도가 더 빠름**
- 애플리케이션 내의 스레드 수를 **풀 크기에 맞춰 제한 가능**
- 작업 수행 로직과 스레드 생성 로직을 분리함으로써 **다양한 실행 전략 적용 가능**
  - 예: 특정 시간 이후 실행, 주기적 실행 등


# Java Thread Pools

## 개요
- Java의 `java.util.concurrent` 패키지에 포함된 **Executor 프레임워크**를 통해 지원됨
- `Executors` 클래스에서 제공하는 **팩토리 메서드**들을 사용하여 스레드 풀 생성 가능

## 주요 팩토리 메서드

1. `static ExecutorService newSingleThreadExecutor()`
   - 크기 1의 스레드 풀 생성
   - 단일 스레드를 반복적으로 재사용

2. `static ExecutorService newFixedThreadPool(int size)`
   - 고정 크기의 스레드 풀 생성
   - 지정된 수만큼의 스레드를 유지함

3. `static ExecutorService newCachedThreadPool()`
   - **제한 없는(unbounded)** 스레드 풀 생성
   - 작업이 많아지면 새 스레드를 만들고, 유휴 스레드는 재사용



# OpenMP

## 개요
- **공유 메모리 환경**에서 병렬 프로그래밍을 지원
- C, C++, FORTRAN을 위한 컴파일러 지시문 및 API 제공

## 주요 개념
- **Compiler directives**로 병렬 영역 지정
- **Parallel regions**: 병렬 실행 가능한 코드 블록을 식별

## 스레드 생성
- 사용 가능한 코어 수만큼 스레드 생성
```c
#pragma omp parallel
    printf("Hello, World!\n");  // 각 스레드에서 실행됨
```
## 병렬 for 루프

```c
#pragma omp parallel for 
for (i = 0; i < N; i++) {     c[i] = a[i] + b[i];  // 루프가 코어 단위로 분할 실행됨 }`
```


