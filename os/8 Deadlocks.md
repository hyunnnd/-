
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

### Deadlock Prevention (조건 무효화)

- **교착 상태 예방(Deadlock prevention)**
    
    - 교착 상태 발생의 **필요 조건 중 최소 하나가 성립되지 않도록** 보장하는 방법들의 집합
        
    - 자원 요청 방식에 제한을 두어 조건을 깨뜨림
        
- **교착 상태 회피(Deadlock avoidance)**
    
    - 시스템이 **교착 상태가 발생하지 않는 안전 상태(safe state)**를 **항상 유지**하도록 관리
        
    - 이를 위해 다음과 같은 **추가 정보**가 필요함:
        
        - 현재 각 스레드에 **할당되었거나 사용 가능한 자원**
            
        - 각 스레드가 **미래에 요청하거나 반환할 자원**에 대한 정보

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