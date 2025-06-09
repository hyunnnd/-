
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

- `Circular Wait`: 모든 자원 유형에 대해 전체 순서를 부여하고, 각 스레드가 자원을 증가하는 순서대로 요청하도록 요구함
    - 순환 대기 조건을 무효화하는 것이 가장 일반적인 방식
    - 각 자원(예: 뮤텍스 락)에 고유한 번호를 부여함
    - 자원은 오름차순 순서대로만 요청해야 함  
        예시: `F(first_mutex) = 1`, `F(second_mutex) = 5`  
        → 스레드는 `F(R_j) > F(R_i)`를 만족할 때에만 `R_j`를 요청할 수 있음
        
    - 예시 코드:
        - `thread_one`: 올바른 자원 획득 순서  
            `pthread_mutex_lock(&first_mutex);`  
            `pthread_mutex_lock(&second_mutex);`
            
        - `thread_two`: 잘못된 자원 획득 순서 (허용되지 않음)  
            `pthread_mutex_lock(&second_mutex);`  
            `pthread_mutex_lock(&first_mutex);`


### Deadlock Avoidance

- 자원이 어떻게 요청될지를 추가적으로 알아야 함  
    예: 각 스레드는 각 자원 유형에 대해 최대 몇 개까지 요청할 수 있는지를 사전에 선언함
- 교착 상태 회피 알고리즘은 `자원 할당 상태(resource-allocation state)`를 동적으로 검사하여 순환 대기 조건이 절대 발생하지 않도록 함
- `Resource-allocation state`는 다음 정보로 정의됨
    - 사용 가능한 자원의 수
    - 현재 할당된 자원의 수
    - 각 스레드의 최대 요구량


### Safe State

- 스레드가 사용 가능한 자원을 요청할 때, 시스템은 즉시 할당하더라도 시스템이 `안전 상태(safe state)`를 유지할 수 있는지를 판단해야 함
- `Safe state`: 모든 스레드에 대해 `안전 순서(safe sequence)`가 존재하는 상태
- `Safe sequence`:  
    스레드들의 순서 ⟨T₁, T₂, ..., Tₙ⟩에 대해, 각 Tᵢ가 앞으로 요청할 수 있는 자원이 현재 사용 가능한 자원 + T₁~Tᵢ₋₁까지의 자원을 합쳐서 충족될 수 있다면 안전한 순서
    - 만약 Tᵢ가 필요로 하는 자원이 즉시 제공되지 않더라도, Tᵢ는 앞선 Tⱼ (j < i) 들이 종료될 때까지 기다릴 수 있음    
    - Tⱼ가 종료되면 자원을 반납하고, Tᵢ는 필요한 자원을 얻어 실행 후 종료함
    - Tᵢ가 종료되면 다음 스레드 Tᵢ₊₁가 자원을 획득하여 실행 가능함

### Safe State and Deadlock

- 시스템이 **안전 상태(safe state)**에 있을 경우 → **교착 상태는 발생하지 않음**
- 시스템이 **비안전 상태(unsafe state)**에 있을 경우 → **교착 상태가 발생할 가능성 있음**
- **교착 상태 회피(deadlock avoidance)** 전략은 시스템이 **절대 비안전 상태에 들어가지 않도록 보장**하는 방식임


### Avoidance Algorithm

- **자원 유형이 단일 인스턴스일 경우**  
    → **Resource-Allocation Graph(자원 할당 그래프)**를 사용하여 교착 상태 회피
    
- **자원 유형이 다중 인스턴스일 경우**  
    → **Banker’s Algorithm(은행원 알고리즘)**을 사용하여 교착 상태 회피


### Resource-Allocation Graph Algorithm

- `Claim edge` (Tᵢ → Rⱼ):
    - 스레드 Tᵢ가 미래에 자원 Rⱼ를 요청할 수 있음을 나타냄
    - 점선으로 표현됨

- 동작 방식:
    - 스레드가 자원을 실제로 요청할 경우, claim edge는 `request edge`로 전환됨
    - 자원이 반환되면 `assignment edge`는 다시 `claim edge`로 전환됨
    - 모든 claim edge는 시스템 초기 단계에서 미리 선언되어야 함

- **자원 요청은 다음 조건을 만족할 때에만 허용됨**:    
    - 요청 edge를 assignment edge로 변환했을 때 **그래프에 cycle(순환)** 이 발생하지 않아야 함

- (오른쪽 그림)    
    - 위: 안전 상태 (점선은 claim edge)
    - 아래: 요청을 수락했을 때 cycle이 형성되며 `Unsafe State` 발생

![[Pasted image 20250605120104.png]]

### Banker's Algorithm

- **다중 인스턴스 자원**을 다루는 교착 상태 회피 알고리즘임
- **각 스레드는 사전에 자원의 최대 사용량을 선언**해야 함
- 스레드가 자원을 요청할 경우, 시스템은 현재 상태가 **안전 상태**인지 판단  
    → 안전하지 않다면 **요청을 지연(대기)**시킴
- 스레드가 필요한 **모든 자원을 획득하면**, 유한한 시간 내에
    - 자원을 사용하고
    - 작업을 완료한 후
    - **모든 자원을 반환해야 함**


### Data Structures for the Banker’s Algorithm

- $n$: 스레드(thread)의 수
- $m$: 자원(resource) 유형의 수
- `Available`: 길이 $m$의 벡터
    - `Available[j] = k`이면, 자원 유형 $R_j$가 시스템에 $k$개 사용 가능함
- `Max`: $n \times m$ 행렬
    - `Max[i][j] = k`이면, 스레드 $T_i$는 자원 유형 $R_j$를 최대 $k$개까지 요청할 수 있음
- `Allocation`: $n \times m$ 행렬
    - `Allocation[i][j] = k`이면, 현재 스레드 $T_i$는 자원 $R_j$를 $k$개 할당받은 상태임
- `Need`: $n \times m$ 행렬
    - `Need[i][j] = k`이면, 스레드 $T_i$가 작업을 완료하기 위해 자원 $R_j$를 $k$개 더 필요로 함
    - 계산식:
        `Need[i][j] = Max[i][j] - Allocation[i][j]`


### Safety Algorithm

1. `Work`와 `Finish`를 각각 길이 $m$과 $n$의 벡터로 초기화함
    - `Work = Available`
    - 모든 $i$에 대해 `Finish[i] = false`

2. 아래 조건을 모두 만족하는 인덱스 $i$를 찾음:
    - (a) `Finish[i] == false`
    - (b) `Needᵢ ≤ Work` (스레드 $Tᵢ$가 필요한 자원을 현재 사용 가능한 자원으로 만족시킬 수 있는 경우)  
        → 그런 $i$가 없다면 Step 4로 이동

3. - `Work = Work + Allocationᵢ` (자원 회수)
    - `Finish[i] = true`
    - Step 2로 되돌아감

4. - 모든 $i$에 대해 `Finish[i] == true`이면 → 시스템은 **안전 상태**임
    - 그렇지 않으면 → **비안전 상태**

### Resource-Request Algorithm

- `Requestᵢ`는 스레드 $Tᵢ$가 요청하는 자원 벡터임  
    → `Requestᵢ[j] = k`는 스레드 $Tᵢ$가 자원 $R_j$를 $k$개 요청함을 의미

#### 동작 절차:

1. **최대 요청량 초과 여부 확인**
    - `Requestᵢ ≤ Needᵢ`이면 다음 단계로
    - 아니라면 오류 발생 (스레드가 최대 요구량을 초과하여 요청한 경우)

2. **자원 가용성 확인**
    - `Requestᵢ ≤ Available`이면 다음 단계로
    - 아니라면 $Tᵢ$는 대기 (자원이 부족함)

3. **가상 할당 수행 (임시 시뮬레이션)**
	Available = Available − Requestᵢ  
	Allocationᵢ = Allocationᵢ + Requestᵢ  
	Needᵢ = Needᵢ − Requestᵢ
	- 이 상태에서 Safety Algorithm을 실행하여 **안전 상태인지 검사**

3. **결과에 따른 처리**
    - 안전 상태이면 → 요청된 자원을 실제로 할당함
    - 비안전 상태이면 → $Tᵢ$는 대기, 상태는 이전으로 복구됨


![[Pasted image 20250605121257.png]]


![[Pasted image 20250605122152.png]]


### Deadlock Detection

- **시스템이 교착 상태에 들어가는 것을 허용**

#### 탐지 알고리즘 종류

- **자원 유형이 단일 인스턴스일 경우**  
- **자원 유형이 다중 인스턴스일 경우**  
#### Recovery Scheme (복구 방법)

#### Single Instance of Each Resource Type

- **Wait-for graph**:  
    자원 할당 그래프에서 **자원 노드(resource nodes)**를 제거하고,  
    **요청 및 할당 간선을 적절히 병합하여 프로세스 간 대기 관계만 표현**함
- **교착 상태는 오직 wait-for graph에 cycle이 존재할 때에만 발생함**
- 탐지를 위해 시스템은 다음을 수행해야 함:
    - **Wait-for graph를 유지**함
    - **Cycle을 탐지하는 알고리즘을 주기적으로 실행**함  
        → 시간 복잡도: **O(n²)**

#### Detection-Algorithm Usage

- **언제 탐지 알고리즘을 실행할 것인가?**
    - 교착 상태가 얼마나 자주 발생하는가?
    - 교착 상태 발생 시 영향을 받는 스레드 수는 얼마인가?
        
- **자원 할당 요청이 즉시 수락되지 않을 때마다 탐지 알고리즘을 실행할 수 있음**
    - 교착 상태는 일부 스레드가 요청을 했으나 즉시 자원을 할당받지 못할 때 발생함
    - 이 경우, **교착 상태를 유발한 스레드를 찾아낼 수 있음**


## Recovery from Deadlock
#### - Process termination
#### - Resource preemption

## Process Termination

- **Abort all deadlocked processes**
    - 너무 많은 비용이 듬 (Too expensive)

- **Abort one process at a time until the deadlock is eliminated**
    - 우선순위 기준 (Order of priority)
        - 시작 후 경과 시간 / 완료까지 남은 시간
        - 해당 프로세스가 사용한 자원 / 완료에 필요한 자원
        - 종료되어야 할 프로세스 수
        - 프로세스가 대화형(interactive)인지, 일괄 처리(batch)인지

## Resource Preemption

- **Selecting a victim**
    - 비용 최소화 (프로세스가 보유한 자원의 수, 지금까지 소비한 시간 등)
- **Rollback**
    - 선택된 프로세스는 안전 상태로 되돌아가 다시 시작해야 함
- **Starvation**
    - 동일한 프로세스에서 계속 자원이 선점되지 않도록 보장할 수 있는가?


## Several Instances of a Resource Type

- **Available**: 길이 `m`인 벡터로, 각 자원 타입별 사용 가능한 인스턴스 수를 나타냄
- **Allocation**: `n × m` 행렬로, 각 스레드에 현재 할당된 자원 인스턴스 수를 나타냄
- **Request**: `n × m` 행렬로, 각 스레드가 현재 요청 중인 자원 수를 나타냄
    - `Request[i][j] = k`이면, 스레드 `Tᵢ`는 자원 타입 `Rⱼ`의 인스턴스를 `k`개 더 요청 중임

## Detection Algorithm

1. `Work`와 `Finish`를 각각 길이 `m`, `n`인 벡터로 설정하고 초기화:
    - (a) `Work = Available`
    - (b) 각 `i = 1, 2, ..., n`에 대해 `Allocationᵢ ≠ 0`이면 `Finish[i] = false`, 아니면 `Finish[i] = true`

2. 다음 조건을 모두 만족하는 인덱스 `i`를 찾음:
    - (a) `Finish[i] == false`
    - (b) `Requestᵢ ≤ Work`  

해당하는 `i`가 없으면 step 4로 이동  

3. `Work = Work + Allocationᵢ`, `Finish[i] = true`  
2단계로 이동  
4. 어떤 `i`에 대해 `Finish[i] == false`이면 시스템은 데드락 상태임.  
특히, `Finish[i] == false`인 `Tᵢ`는 데드락에 빠진 상태임

- 알고리즘의 시간 복잡도는 `O(m × n²)`이며, 시스템이 데드락 상태인지 판단하는 데 이 정도 연산이 필요함


수업때안하긴함