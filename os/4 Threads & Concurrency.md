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
결과: 10, 20 
	returned 
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
