
## Chapter 8. Deadlocks

### Deadlock State

- 교착 상태란 모든 스레드가 다른 스레드에 의해 발생 가능한 이벤트를 기다리는 상태임
- 여기서 다루는 이벤트는 주로 자원의 획득 및 반환

### Deadlock in Multithreaded Application

```c
pthread_mutex_t first_mutex;
pthread_mutex_t second_mutex;

pthread_mutex_init(&first_mutex, NULL);
pthread_mutex_init(&second_mutex, NULL);

void *do_work_one(void *param) {
    pthread_mutex_lock(&first_mutex);
    pthread_mutex_lock(&second_mutex);
    // 작업 수행
    pthread_mutex_unlock(&second_mutex);
    pthread_mutex_unlock(&first_mutex);
    pthread_exit(0);
}

void *do_work_two(void *param) {
    pthread_mutex_lock(&second_mutex);
    pthread_mutex_lock(&first_mutex);
    // 작업 수행
    pthread_mutex_unlock(&first_mutex);
    pthread_mutex_unlock(&second_mutex);
    pthread_exit(0);
}
```
![[Pasted image 20250605113254.png]]
### Deadlock Characterization (교착 상태 조건)

- 네 가지 조건이 동시에 성립될 때 교착 상태 발생 가능
    - **상호 배제(Mutual exclusion)**: 하나의 스레드만 자원 사용 가능
    - **보유와 대기(Hold and wait)**: 최소 하나의 자원을 보유한 상태로 다른 자원을 요청
    - **비선점(No preemption)**: 자원은 자발적으로 반환될 때만 회수 가능
    - **순환 대기(Circular wait)**: 스레드들이 원형으로 자원을 기다리는 상태
### Resource-Allocation Graph

- 교착 상태는 **자원-할당 그래프(resource-allocation graph)**라는 **방향 그래프**를 통해 더 명확하게 설명할 수 있습니다.
- 그래프는 정점 집합 V와 간선 집합 E로 구성됩니다.
- **정점 집합 V**는 두 가지 유형의 정점으로 구성됩니다:
    - T={T1,T2,...,Tn}T = \{T_1, T_2, ..., T_n\}T={T1​,T2​,...,Tn​}: 스레드 집합
    - R={R1,R2,...,Rm}R = \{R_1, R_2, ..., R_m\}R={R1​,R2​,...,Rm​}: 자원 종류 집합
- **간선 집합 E**도 두 가지 방향 간선으로 나뉩니다:
    - Ti→RjT_i \rightarrow R_jTi​→Rj​: **요청(request) 간선**
    - Rj→TiR_j \rightarrow T_iRj​→Ti​: **할당(assignment) 간선**
- 요청 간선이 만족되면 해당 간선은 할당 간선으로 변경됩니다.
![[Pasted image 20250602122345.png]]
### Resource-Allocation Graph Example

- R1: 1개, R2: 2개, R3: 1개, R4: 3개
- T1은 R2를 보유, R1 요청 중
- T2는 R1, R2 보유, R3 요청 중
- T3은 R3 보유 중

### Graph 분석

- 사이클 없으면 교착 상태 없음
- 사이클 있고 자원 인스턴스가 하나씩이면 교착 상태
- 자원 인스턴스가 여러 개면 교착 상태 가능성 있음

### Deadlock 처리 방법

- 무시: 시스템에서 교착 상태가 발생하지 않는다고 가정
- 예방 또는 회피: 시스템이 교착 상태에 빠지지 않도록 보장
- 허용 및 복구: 교착 상태를 탐지하고 회복 절차 수행


### Deadlock Prevention

- 교착 상태 발생의 네 가지 필요 조건 중 하나를 무효화하여 교착 상태 방지
- `Mutual exclusion`: 공유 가능한 자원(예: 읽기 전용 파일)에 대해서는 필요하지 않음
    - 공유 불가능한 자원에 대해서는 반드시 만족해야 함
    - 많은 자원이 본질적으로 공유 불가능함
    
- `Hold and wait`: 스레드가 자원을 요청할 때, 다른 자원을 보유하고 있지 않도록 보장
    - 실행 전에 모든 필요한 자원을 한 번에 할당하거나
    - 어떤 자원도 보유하지 않은 상태에서만 자원 요청을 허용
    - 단점: 자원 활용률 저하 및 기아(starvation) 가능성

- `No Preemption`: 자원의 선점 불가 조건을 무효화함으로써 교착 상태 방지    
    - 스레드가 자원을 보유한 상태에서 새로운 자원을 요청했을 때, 해당 자원이 즉시 할당되지 않으면 현재 보유 중인 모든 자원을 반납함
    - 반납된 자원은 스레드가 기다리는 자원 목록에 추가됨
    - 스레드는 이전에 보유했던 자원들과 새로 요청한 자원들을 모두 다시 획득할 수 있을 때에만 다시 실행됨


