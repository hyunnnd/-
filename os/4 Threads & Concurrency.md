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