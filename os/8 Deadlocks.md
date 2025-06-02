
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

### Deadlock Characterization (교착 상태 조건)

- 네 가지 조건이 동시에 성립될 때 교착 상태 발생 가능
    
    - **상호 배제(Mutual exclusion)**: 하나의 스레드만 자원 사용 가능
    - **보유와 대기(Hold and wait)**: 최소 하나의 자원을 보유한 상태로 다른 자원을 요청
    - **비선점(No preemption)**: 자원은 자발적으로 반환될 때만 회수 가능
    - **순환 대기(Circular wait)**: 스레드들이 원형으로 자원을 기다리는 상태
### Resource-Allocation Graph

- 교착 상태를 설명하는 방향 그래프
- 정점: T (스레드), R (자원)
- 간선: T → R (요청), R → T (할당)
- 요청 간선이 만족되면 할당 간선으로 변경됨
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
    

### Deadlock Prevention (조건 무효화)

- **상호 배제**: 공유 자원에만 필요, 대부분 자원은 비공유
    
- **보유와 대기**: 실행 전 모든 자원 할당 또는 자원을 보유하지 않은 상태에서만 요청  
    → 자원 활용도 낮고 기아 상태 발생 가능
    
- **비선점**: 새 자원이 즉시 할당되지 않으면 기존 자원 반환
    
- **순환 대기**: 자원에 순서를 지정하고 그 순서대로 요청하도록 제한
    

```c
// 자원 순서 F(first_mutex)=1, F(second_mutex)=5 일 때
// 올바른 순서로만 요청 가능
```

### Deadlock Avoidance (회피)

- 자원 요청 방식을 미리 선언 (ex. 최대 자원 수)
    
- 시스템 상태를 실시간 검사하여 안전 상태 유지
    
    - 가용 자원 수, 현재 할당 상태, 각 스레드의 최대 요구량
        

### Safe State

- 안전 상태: 모든 스레드가 순서대로 종료할 수 있는 자원 할당 상태
    
- 안전 시퀀스 존재 여부로 판단
    
- 안전하지 않은 상태는 교착 상태 가능성 있음
    

### Avoidance Algorithm

- 자원 1개씩: 자원-할당 그래프 사용
    
- 자원 여러 개: Banker’s Algorithm 사용
    

### Banker’s Algorithm 데이터 구조

- n = 스레드 수, m = 자원 종류 수
    
- Available[m], Max[n][m], Allocation[n][m], Need[n][m]
    
    - Need[i][j] = Max[i][j] - Allocation[i][j]
        

### Safety Algorithm

1. Work = Available, Finish[i] = false
    
2. Finish[i]==false 이고 Need[i] ≤ Work 인 i 찾기
    
3. Work += Allocation[i], Finish[i] = true
    
4. 모든 i에 대해 Finish[i]==true이면 안전 상태
    

### Resource-Request Algorithm

- 요청이 Need와 Available을 초과하면 오류 또는 대기
    
- 시스템이 안전하면 할당, 아니면 원상 복구 후 대기
    

### Banker’s Algorithm 예시

- 자원: A=10, B=5, C=7 / 스레드: T0~T4
    
- 안전 시퀀스 존재 시 안전 상태 (<T1, T3, T4, T2, T0> 등)
    

### Deadlock Detection (탐지)

- 시스템이 교착 상태에 들어가는 것을 허용하고 이후 탐지
    
- **자원 1개일 경우**: Wait-for 그래프 사용 (사이클 존재 여부로 판단)
    
- 탐지 알고리즘은 요청이 거절될 때 수행 가능
    

### Recovery from Deadlock (복구)

- 프로세스 종료
    
    - 전부 종료: 비용 큼
        
    - 하나씩 종료: 우선순위 기반 결정
        
- 자원 선점
    
    - 희생자 선정 (비용 최소화)
        
    - 롤백 수행
        
    - 기아 방지 고려 필요
        

### 추가: 자원 여러 개일 때 탐지 알고리즘

1. Work, Finish 벡터 초기화
    
2. Request[i] ≤ Work 인 i 찾기
    
3. Work += Allocation[i], Finish[i]=true
    
4. 남은 Finish[i]==false 존재 시 교착 상태, 해당 Ti는 교착
    

### 탐지 알고리즘 예시

- 자원: A=7, B=2, C=6 / 스레드: T0~T4
    
- 교착 상태 확인: T1, T2, T3, T4 가 Finish[i]==false
    

---

※ 자료 출처: Operating Systems: Three Easy Pieces 및 KAIST OSTEP 슬라이드