
### 세마포어

- 세마포어: 프로세스가 활동을 동기화할 수 있도록 돕는 도구 (뮤텍스보다 더 정교한 방식 제공)
- 세마포어 S는 정수형 변수이며 두 가지 원자적 연산만 가능함
    - `wait()` 연산
    - `signal()` 연산

```c
wait(S) {
    while (S <= 0);
    S--;
}

signal(S) {
    S++;
}
```

### 세마포어의 용도

- 카운팅 세마포어: 값이 제한 없이 가능하며, 자원의 수로 초기화됨
- 이진 세마포어: 값이 0 또는 1 사이이며, 뮤텍스 락과 동일
- 다양한 동기화 문제 해결에 사용 가능함

### POSIX 세마포어

- 이름 있는 세마포어: 여러 비관련 프로세스가 쉽게 공유 가능
- 생성 및 초기화, 획득 및 해제 예시:

```c
#include <semaphore.h>
sem_t *sem;
sem = sem_open("SEM", O_CREAT, 0666, 1);

sem_wait(sem);
/* critical section */
sem_post(sem);
```

- `sem_wait()`: 값이 1 이상이면 즉시 반환, 0 이하면 대기
- `sem_post()`: 값을 증가시키며, 대기 중인 쓰레드가 있으면 하나를 깨움

### 이름 없는 세마포어

```c
#include <semaphore.h>
sem_t sem;
sem_init(&sem, 0, 1);

sem_wait(&sem);
/* critical section */
sem_post(&sem);
```

- `pshared = 0`이면 프로세스 내 쓰레드 공유, 아니면 프로세스 간 공유

### 이진 세마포어

- Using a semaphore as a lock
- The initial value should be 1
```c
sem_t m;
sem_init(&m, 0, 1);

sem_wait(&m);
// critical section
sem_post(&m);
```

### 순서를 위한 세마포어

```c
sem_t s;
void *child(void *arg) {
    printf("child\n");
    sem_post(&s);
    return NULL;
}
int main(...) {
    sem_init(&s, 0, 0);
    printf("parent: begin\n");
    pthread_create(..., child, NULL);
    sem_wait(&s);
    printf("parent: end\n");
}
```

### 제한 버퍼 문제

- 버퍼가 가득 차면 생산자는 소비자가 아이템을 제거할 때까지 대기해야 함
- 버퍼가 비면 소비자는 생산자가 아이템을 추가할 때까지 대기해야 함
- empty, full 세마포어 사용

### 세마포어를 사용한 제한 버퍼 구현

##### Imagine that MAX is greater than 1.
 - 생산자와 소비자가 여러 명일 경우, 경쟁 조건(race condition)이 발생할 수 있음
- 이는 기존 데이터가 덮어써지는 상황을 의미함
- 버퍼에 값을 채우고 인덱스를 증가시키는 동작은 임계 구역(critical section)에 해당함

![[Pasted image 20250602114955.png]]

### 뮤텍스 추가 해결 방안

![[Pasted image 20250602115056.png]]

- 뮤텍스를 잘못 사용하는 경우 교착 상태 발생
- 작동하는 해결책: 뮤텍스는 `put()`과 `get()` 전에, `post()` 후에 사용
- 생산자 하나와 소비자 하나의 두 개의 스레드를 가정함
- 소비자가 뮤텍스를 획득함 (C0)
- 소비자가 `full` 세마포어에 대해 `sem_wait()`을 호출함 (C1)
- 소비자는 블록되어 CPU를 양보함
- 하지만 여전히 뮤텍스를 보유하고 있음
- 생산자가 바이너리 뮤텍스 세마포어에 대해 `sem_wait()`을 호출함 (P0)
- 생산자도 대기 상태에 빠짐 → 전형적인 교착 상태 발생

![[Pasted image 20250602115114.png]]

### 리더-라이터 문제

- 여러 리더와 라이터가 공유 데이터 접근
- 리더는 동시에 접근 가능하지만, 라이터는 단독 접근 필요

### 리더-라이터 락

- 삽입(insert) 및 단순 조회(lookup)를 포함하는 여러 개의 동시 리스트 연산을 가정함
	- **삽입 (insert):** 리스트의 상태를 변경함 → 전통적인 임계 구역 사용이 적절함
	- **조회 (lookup):** 단순히 자료구조를 읽기만 함
	    - 삽입이 진행되지 않는다는 보장만 있다면 여러 개의 조회가 동시에 수행되어도 무방함
- 이러한 특별한 형태의 락을 **리더-라이터 락(reader-writer lock)**이라 부름

- 오직 하나의 라이터만 락을 획득할 수 있음
- 리더가 읽기 락을 획득하면, 추가적인 리더들도 동시에 락을 획득할 수 있음
- 라이터는 모든 리더가 작업을 마칠 때까지 대기해야 함x`

![[Pasted image 20250602115226.png]]
### 리더 락/해제, 라이터 락/해제 함수

```c
void rwlock_acquire_readlock(...) {
    sem_wait(&rw->lock);
    rw->readers++;
    if (rw->readers == 1)
        sem_wait(&rw->writelock);
    sem_post(&rw->lock);
}

void rwlock_release_readlock(...) {
    sem_wait(&rw->lock);
    rw->readers--;
    if (rw->readers == 0)
        sem_post(&rw->writelock);
    sem_post(&rw->lock);
}

void rwlock_acquire_writelock(...) {
    sem_wait(&rw->writelock);
}

void rwlock_release_writelock(...) {
    sem_post(&rw->writelock);
}
```

- 공정성 문제 존재: 리더가 라이터를 굶기게 될 수 있음

### 식사하는 철학자 문제

**문제 정의**

- 5명의 철학자가 원형 테이블에 앉아 있음    
    - 생각하거나 식사함
- 5개의 밥그릇과 5개의 단일 젓가락
- 동료와의 상호작용 없음
- 식사를 하기 위해 철학자는 자기 양옆의 두 젓가락을 들어야 함
- 철학자는 한 번에 하나의 젓가락만 집을 수 있음
- 식사를 마친 후 젓가락을 내려놓음

**해결책의 조건**

- 교착 상태(deadlock)가 없어야 하며
- 굶주림(starvation)도 없어야 함

### 기본 해결책 (데드락 발생 가능)

- **공유 데이터**    
    - 밥그릇 (데이터 집합)
    - `chopstick[5]`이라는 세마포어 배열, 각 젓가락은 1로 초기화됨

- **가능한 해결책**
    - 각 철학자가 자신의 왼쪽 젓가락을 먼저 집는다면,
    - 그 누구도 오른쪽 젓가락을 집지 못하게 되고,
    - 각자 하나의 젓가락만 쥔 채로 영원히 기다리는 **데드락** 상태 발생

```c
sem_wait(chopstick[i]);
sem_wait(chopstick[(i+1) % 5]);
/* eat */
sem_post(chopstick[i]);
sem_post(chopstick[(i+1) % 5]);
```

### 해결책: 의존성 끊기

```c
if (p == 4) {
    sem_wait(chopstick[(i+1) % 5]);
    sem_wait(chopstick[i]);
} else {
    sem_wait(chopstick[i]);
    sem_wait(chopstick[(i+1) % 5]);
}
```

- 혹은 최대 4명의 철학자만 앉도록 제한하거나, 두 젓가락 모두 가능할 때만 집게 함
    
- 홀수/짝수 철학자 접근 순서 변경
    

### 참고자료

- Operating Systems: Three Easy Pieces Ch31
    
- [https://oslab.kaist.ac.kr/ostepslides/](https://oslab.kaist.ac.kr/ostepslides/)