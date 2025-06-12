
## Concepts

- **File**
    - 파일은 바이트들의 선형 배열임        
    - 각 파일은 내부적으로 `inode number`라는 저수준 이름을 가짐

- **Directory**
    - 디렉터리도 하나의 파일임
    - `<사용자 친화적 파일명, 내부 파일명>` 쌍의 리스트로 구성됨
    - 예시 경로:
        - valid files: `/foo/bar.txt`, `/bar/foo/bar.txt`
        - valid directories: `/`, `/foo`, `/bar`, `/bar/bar`, `/bar/foo/`


빨간글씨 위주


## Open File Table

- 자식 프로세스는 부모의 파일 디스크립터 테이블(file descriptor table)을 상속받음

### 예제 코드 설명
```c
int main(int argc, char *argv[]) {
    int fd = open("file.txt", O_RDONLY);
    assert(fd >= 0);

    int rc = fork();
    if (rc == 0) { // 자식 프로세스
        rc = lseek(fd, 10, SEEK_SET);
        printf("child: fd %d, offset %d\n", fd, rc);
    } else if (rc > 0) { // 부모 프로세스
        (void) wait(NULL);
        printf("parent: fd %d, offset %d\n", fd,
               (int) lseek(fd, 0, SEEK_CUR));
    }
    return 0;
}
```

### 출력 결과

`child: fd 3, offset 10 parent: fd 3, offset 10`


### 설명
- 부모와 자식 프로세스는 각각 독립적인 **file descriptor table**을 가짐 (fd 3)    
- 그러나 이들은 동일한 **open file table entry**를 참조함  
    → `refcnt = 2`로 참조 횟수 2회  
    → `offset`도 공유되며 `file.txt`의 inode를 가리킴
- 즉, **`lseek()` 호출은 offset을 공유하는 구조**로 작동하여 부모와 자식 모두 같은 위치(10)를 참조하게 됨


## fsync()

- **지속성(Persistency)**
  - `write()` 함수는 데이터를 커널의 버퍼에만 기록하고, 나중에 디스크에 저장함
  - 하지만 일부 응용프로그램(DBMS 등)은 반드시 즉시 디스크에 저장되길 요구함

- **fsync() 함수**  
  - 디스크에 즉시 쓰기를 강제하는 함수  
  - 파일의 메모리 상(in-core)의 상태를 저장 장치 상태와 동기화함

```c
#include <unistd.h>
int fsync(int fd);
```

- **예제**
`int fd = open("foo", O_CREAT | O_WRONLY | O_TRUNC);` 
`int rc = write(fd, buffer, size); `
`rc = fsync(fd);`


## stat()

- `stat()`: 파일의 상태 정보를 가져오는 함수

```c
#include <sys/stat.h>

int stat(const char *restrict pathname, struct stat *restrict statbuf);
int fstat(int fd, struct stat *statbuf);
int lstat(const char *restrict pathname, struct stat *restrict statbuf);
```
stat(): 주어진 경로(pathname)가 가리키는 실제 파일에 대한 정보를 반환함
fstat(): stat()과 동일하나, 경로 대신 파일 디스크립터(fd)를 통해 파일 정보를 얻음
lstat(): stat()과 거의 동일하지만, pathname이 심볼릭 링크일 경우 링크 자체의 정보를 반환함 (링크가 가리키는 파일이 아님)
statstructure: file status

## stat() 사용 예제

### 📌 목적
파일의 상태 정보(크기, inode 번호, 권한, 소유자, 접근 시간 등)를 출력하는 프로그램

### 🧾 주요 코드
```c
#include <stdio.h>
#include <sys/stat.h>
#include <unistd.h>
#include <time.h>

int main(int argc, char *argv[]) {
    if (argc != 2) {
        fprintf(stderr, "Usage: %s <filename>\n", argv[0]);
        return 1;
    }

    const char *filename = argv[1];
    struct stat file_stat;

    if (stat(filename, &file_stat) == -1) {
        perror("stat");
        return 1;
    }

    // File status 출력
    printf("File: %s\n", filename);
    printf("Size: %ld bytes\n", file_stat.st_size);
    printf("Inode: %ld\n", file_stat.st_ino);
    printf("Permissions: %o\n", file_stat.st_mode & 0777);
    printf("Links: %ld\n", file_stat.st_nlink);
    printf("Owner UID: %d\n", file_stat.st_uid);
    printf("Group GID: %d\n", file_stat.st_gid);
    printf("Last accessed: %s", ctime(&file_stat.st_atime));
    printf("Last modified: %s", ctime(&file_stat.st_mtime));
    printf("Last status change: %s", ctime(&file_stat.st_ctime));

    return 0;
}
```

### 💡 실행 예시
```
$ ./filestat file 
File: file 
Size: 6 bytes 
Inode: 811949 
Permissions: 664 
Links: 1 
Owner UID: 1000 
Group GID: 1000 
Last accessed: Sat Jun  7 23:08:34 2025 
Last modified: Sat Jun  7 23:08:34 2025 
Last status change: Sat Jun  7 23:08:34 2025
```

### 🔎 참고

- `stat()` 함수는 파일의 메타데이터를 구조체 `stat`에 저장함
- `ctime()`을 통해 시간 정보는 사람이 읽을 수 있는 문자열로 변환됨


## stat().st_mode

- `st_mode` 필드는 파일의 **종류(type)**와 **권한(permission)** 정보를 모두 포함함
- 파일 종류 확인을 위해 비트 마스크 값 사용

### 📌 파일 종류 구분용 마스크 값

| 매크로      | 값       | 설명             |
|-------------|----------|------------------|
| `S_IFMT`    | 0170000  | 파일 타입 마스크 (bit 필드 마스크) |
| `S_IFSOCK`  | 0140000  | 소켓             |
| `S_IFLNK`   | 0120000  | 심볼릭 링크       |
| `S_IFREG`   | 0100000  | 일반 파일        |
| `S_IFBLK`   | 0060000  | 블록 디바이스     |
| `S_IFDIR`   | 0040000  | 디렉토리         |
| `S_IFCHR`   | 0020000  | 문자 디바이스     |
| `S_IFIFO`   | 0010000  | FIFO (파이프)     |

### 🔍 파일 타입 판별 예시

#### 방법 1: 비트 마스크 비교
```c
stat(pathname, &sb);
if ((sb.st_mode & S_IFMT) == S_IFREG) {
    // 일반 파일 처리
}
```
#### 방법 2: 헬퍼 매크로 사용 (`<sys/stat.h>` 제공)
```c
stat(pathname, &sb); 
if (S_ISREG(sb.st_mode)) {     // 일반 파일 처리 

} 
if (S_ISDIR(sb.st_mode)) {     // 디렉토리 처리 

}
```
`S_ISREG`, `S_ISDIR` 등의 매크로는 가독성이 높고 안전하게 파일 타입을 검사하는 데 유용함


## stat().st_mode – 파일 권한 마스크
## stat().st_mode – 파일 권한 마스크

### 📌 Special Permission Bits
| 매크로      | 값      | 설명                              |
|-------------|---------|-----------------------------------|
| S_ISUID     | 04000   | set-user-ID 비트 (exec 시 사용자 권한 유지) |
| S_ISGID     | 02000   | set-group-ID 비트                  |
| S_ISVTX     | 01000   | sticky bit (디렉토리 내 파일 삭제 제한 등)  |

### 📌 User (소유자) Permission
| 매크로      | 값     | 설명                          |
|-------------|--------|-------------------------------|
| S_IRWXU     | 00700  | 소유자 읽기/쓰기/실행 권한 전체 |
| S_IRUSR     | 00400  | 소유자 읽기 권한                |
| S_IWUSR     | 00200  | 소유자 쓰기 권한                |
| S_IXUSR     | 00100  | 소유자 실행 권한                |

### 📌 Group Permission
| 매크로      | 값     | 설명                          |
|-------------|--------|-------------------------------|
| S_IRWXG     | 00070  | 그룹 읽기/쓰기/실행 권한 전체   |
| S_IRGRP     | 00040  | 그룹 읽기 권한                  |
| S_IWGRP     | 00020  | 그룹 쓰기 권한                  |
| S_IXGRP     | 00010  | 그룹 실행 권한                  |

### 📌 Others Permission
| 매크로      | 값     | 설명                            |
|-------------|--------|---------------------------------|
| S_IRWXO     | 00007  | 기타 사용자 읽기/쓰기/실행 권한 전체 |
| S_IROTH     | 00004  | 기타 사용자 읽기 권한              |
| S_IWOTH     | 00002  | 기타 사용자 쓰기 권한              |
| S_IXOTH     | 00001  | 기타 사용자 실행 권한              |

### 💡 예시 (`ls -al` 출력 해석)
-rwxrwxr-x → 소유자(rwx), 그룹(rwx), 기타 사용자(r-x)  
-rw-rw-r-- → 소유자(rw-), 그룹(rw-), 기타 사용자(r--)

- 각 비트는 `st_mode`의 하위 9비트에 저장되어 있음
- 이 정보를 `&` 연산자로 마스크 처리하여 필요한 권한을 확인할 수 있음




## opendir() & readdir()

- `opendir()`: 디렉토리를 여는 함수  
- `readdir()`: 디렉토리에서 엔트리를 하나씩 읽는 함수  

- 디렉토리도 파일의 일종이지만, 특정한 구조를 가짐
- 디렉토리를 읽을 때는 일반적인 `read()`가 아닌 전용 시스템 호출을 사용함

### 주요 함수 원형
- `DIR *opendir(const char *name);`  
- `struct dirent *readdir(DIR *dirp);`  
- `int closedir(DIR *dirp);`  
- `DIR *`: 디렉토리 스트림을 가리키는 포인터

### 예제 코드

```C
    int main(int argc, char *argv[]) {
        DIR *dp = opendir("."); // 현재 디렉토리 열기
        assert(dp != NULL);
        struct dirent *d;
        while ((d = readdir(dp)) != NULL) { // 디렉토리 엔트리 하나씩 읽기
            printf("%d %s\n", (int) d->d_ino, d->d_name);
        }
        closedir(dp); // 디렉토리 닫기
        return 0;
    }
    
```

### 참고 링크
- https://man7.org/linux/man-pages/man3/opendir.3.html
- https://man7.org/linux/man-pages/man3/readdir.3.html

![[Pasted image 20250612114222.png]]


## link()

- `link()`: 파일의 새로운 이름을 생성함 (즉, 하드 링크를 생성)
- 하드 링크를 사용하면 하나의 파일에 대해 여러 이름이 존재할 수 있음

### 📌 예시: 하드 링크 생성

```sh
echo hello > file        # 파일 file 생성
cat file                 # 내용 확인 → hello
ln file file2            # file2라는 하드 링크 생성
ls -l                    # file, file2 동일한 크기/시간/퍼미션
cat file2                # file2 읽기 → hello
ls -i file file2         # inode 번호 확인 → 동일한 inode (811752)
ln file file2 명령어는 file2라는 이름으로 file과 같은 inode를 참조하는 새 링크 생성
```
즉, file과 file2는 다른 이름이지만 같은 파일을 가리킴

이때 두 파일의 내용, 권한, 소유자 등 모든 것이 동일하며 구분 불가능

⚠️ 파일 삭제와 unlink()
하드 링크는 inode의 참조 수를 증가시킴

따라서 unlink()를 호출하거나 rm으로 지워도 참조 수가 0이 되지 않으면 파일은 실제 삭제되지 않음

inode의 참조 수가 0이 되어야 디스크 공간이 해제됨

🔗 참고 링크
https://man7.org/linux/man-pages/man2/link.2.html
필요에 따라 셸 명령 부분은 `sh` 블록 또는 그냥 `bash`로 바꿔도 무방합니다.



## unlink()

- `unlink()`: 파일 이름을 삭제하며, 필요 시 실제 파일도 제거
### 함수 원형

`#include <unistd.h> int unlink(const char *pathname);`

### 동작 설명

- `unlink()`는 파일 시스템에서 이름을 제거함 (`rm` 명령어 내부에서 호출됨)
- 해당 이름이 **마지막 링크이며 열려 있는 프로세스가 없을 경우**, 파일은 삭제되고 디스크 공간이 회수됨
- 해당 이름이 **마지막 링크이지만 여전히 열려 있는 프로세스가 있을 경우**, 그 파일은 해당 프로세스들이 닫을 때까지 유지됨
- 심볼릭 링크인 경우에는 **링크만 제거되고 원본 파일은 남아 있음**


## unlink()의 동작 원리

- `unlink()`는 다음과 같은 작업을 수행함:
  - 해당 inode의 **참조 카운트(reference count)** 확인
  - **사람이 읽을 수 있는 이름**과 **inode 번호** 사이의 링크 제거
  - 참조 카운트를 **1 감소**시킴  
    → 참조 카운트가 0이 되었을 때 inode와 데이터 블록을 해제함

### 요약
- 파일 이름 삭제 = 링크 제거
- 참조 카운트가 0이 되어야 실제 데이터가 삭제됨
- 하나의 파일을 가리키는 하드 링크가 여러 개 존재할 수 있음

### 예시 실습
```sh
echo hello > file         # 파일 생성
stat file                 # inode: 811752, Links: 1

ln file file2             # file2 하드 링크 생성
stat file                 # Links: 2
stat file2                # 동일 inode, Links: 2

ln file2 file3            # file3 하드 링크 생성
stat file                 # Links: 3

rm file                   # file 삭제 → Links: 2
stat file2                # Links: 2

rm file2                  # file2 삭제 → Links: 1
stat file3                # Links: 1

rm file3                  # 마지막 링크 삭제 → 실제 파일 삭제
```

`file`, `file2`, `file3`는 모두 같은 inode를 가리키며 삭제될 때마다 Links 수가 감소하다가 마지막에 삭제됨


## Symbolic Links

- **Symbolic link (symlink)**: 실제 파일의 경로를 포함하는 특수 파일
- 일반 파일이 아닌 참조용 경로를 저장
- 하드 링크와 달리 다음 제한이 없음:
  - **디렉토리**에도 링크 가능
  - **다른 파티션**의 파일에도 링크 가능

### 특징 요약
- `ln -s [원본] [링크이름]` 형태로 생성 (`-s` 옵션은 symbolic 링크)
- 심볼릭 링크는 새로운 inode를 가짐 (원본과 inode 다름)
- 원본이 삭제되면 **dangling link**가 되어 접근 불가

### 예제
```sh
echo hello > file           # 일반 파일 생성
ln -s file file2            # file2는 file을 참조하는 심볼릭 링크
ls -l                       # file2 -> file 표시됨
ls -i file file2            # 서로 다른 inode 확인 가능
cat file2                   # file을 통해 내용 읽기 가능 (hello)
rm file                     # 원본 삭제
cat file2                   # 오류 발생: No such file or directory
```

file2는 file의 경로만 참조하므로, file이 삭제되면 파일 내용 접근 불가


![[Pasted image 20250612115421.png]]


## Secondary Storages

- 현대 컴퓨터의 보조 저장 장치(secondary storage)의 대부분은 다음과 같음:
  - **HDDs (Hard Disk Drives)**  
  - **NVM (Nonvolatile Memory)**, 대표적으로 SSD
### 🧭 HDD 구조

- 회전하는 자기 디스크(platter)에 데이터를 저장
- 주요 구성:
  - **Platter**: 원형 자기 디스크
  - **Spindle**: 플래터 회전을 담당
  - **Track (t)**: 플래터 원형 경로
  - **Sector (s)**: 트랙을 나눈 가장 작은 물리적 단위
  - **Cylinder (c)**: 여러 플래터의 같은 트랙 번호를 수직으로 연결
  - **Read-write head**: 실제 데이터를 읽고 쓰는 장치
  - **Arm**: 헤드를 트랙 위치에 맞게 이동
  - **Arm assembly**: 다수의 arm과 헤드를 제어

### 🔋 SSD 구조 (NVM 기반)

- **기계적 움직임 없음**: 전자 회로 기반의 비휘발성 저장소
- 슬라이드 예시: 3.5-inch SSD 회로 보드
- 내부는 **NAND 플래시 메모리**로 구성

#### NAND Block 상태
- SSD는 블록 단위로 데이터를 관리하며 페이지 단위로 기록함
- 하나의 NAND 블록 내에는 다음과 같은 페이지들이 존재:
  - `valid page`: 유효한 데이터가 존재함
  - `invalid page`: 덮어쓰기로 인해 폐기된 상태

> SSD는 덮어쓰기가 불가능하므로, 새 데이터는 새 위치에 기록되고 기존 페이지는 invalid로 처리됨. 이로 인해 가비지 컬렉션이 필요함.


## Disk Drives Address Mapping

- 디스크 드라이브는 **논리 블록(logical block)**의 일차원 배열로 주소가 지정됨
  - 논리 블록은 전송 단위의 최소 단위
  - 로우 레벨 포맷(Low-level formatting)은 물리적 매체 위에 논리 블록을 생성함

### 🧭 논리 블록 → 물리 위치 매핑

- 논리 블록 배열은 디바이스의 **섹터(sector)** 또는 **페이지(page)**에 매핑됨

#### 🔹 HDD (하드 디스크)의 경우
- **Sector 0**: 가장 바깥쪽 실린더의 첫 번째 트랙의 첫 번째 섹터
- 매핑 순서:
  - 해당 트랙의 모든 섹터 → 트랙 내 이동
  - 같은 실린더 내 나머지 트랙들
  - 외부 → 내부 방향으로 다음 실린더들 반복

#### 🔹 NVM (예: SSD)의 경우
- 매핑은 `(chip, block, page)` 튜플을 논리 블록 배열로 변환하는 방식
- 내부 구조는 다르지만 논리 블록으로 일관된 주소를 제공

### ✅ LBA (Logical Block Addressing)
- LBA는 다음 이유로 널리 사용됨:
  - **알고리즘이 처리하기 쉬움**
  - `(sector, cylinder, head)` 또는 `(chip, block, page)` 방식보다 단순

> 즉, 디스크의 내부 복잡도와 무관하게 논리 주소만으로 접근 가능하게 하는 것이 LBA임


## Disk Structure

- 디스크는 **partition(파티션)** 으로 나눌 수 있음
- 디스크나 파티션은 다음과 같이 사용 가능:
  - **raw 상태**: 파일 시스템 없이 사용
  - **formatted 상태**: 파일 시스템으로 포맷하여 사용
- 파티션은 **minidisk**, **slice** 등의 이름으로도 불림
- 파일 시스템이 포함된 개체는 **volume(볼륨)** 이라고 부름
- 각 볼륨은 해당 파일 시스템의 정보를 다음에 저장함:
  - **device directory**
  - **volume table of contents**
- 운영 체제나 컴퓨터 안에는 다음과 같은 다양한 파일 시스템이 공존할 수 있음:
  - **general-purpose file systems** (범용 파일 시스템)
  - **special-purpose file systems** (특수 목적 파일 시스템)


## Storage Device Organization

- 범용 컴퓨터는 **여러 개의 저장 장치**(storage devices)를 가질 수 있음

### 🔹 저장 장치 구성 방식
- 하나의 장치는 **여러 파티션(partition)**으로 나눌 수 있음
- 각 파티션은 **볼륨(volume)**을 포함하며, 하나의 볼륨이 **여러 파티션에 걸쳐 있을 수도 있음**
- 볼륨은 보통 **파일 시스템**으로 포맷되어 있음
- 운영체제는 다양한 유형의 **파일 시스템**을 지원 (수십 종의 파일 시스템이 존재 가능)

---

### 📂 구성 예시

#### 저장 장치 1 (storage device 1)
- partition A → volume 1 → 파일 시스템 및 디렉토리
- partition B → volume 2 → 파일 시스템 및 디렉토리

#### 저장 장치 2, 3 (storage device 2 & 3)
- partition C, D가 함께 volume 3을 구성 → 하나의 파일 시스템으로 운영

> 하나의 volume은 반드시 하나의 partition에만 있는 것은 아니며, 여러 장치를 걸쳐 있을 수 있음 (특히 RAID나 LVM에서 중요함)



### 🔍 요약

- `mount` 명령어를 사용하여 파일 시스템을 트리 구조의 특정 디렉토리에 연결
- 마운트 포인트 아래의 기존 내용은 **새로운 파일 시스템이 마운트되면 보이지 않게 됨**
- 루트(`/`) 디렉토리부터 시작하여 계층적으로 마운트됨

> 마운트는 파일 시스템 간 통합된 트리 구조를 구성하는 데 필수적인 절차임




## File System Layers

### 📚 계층 구조의 목적
- 파일 시스템의 계층화는 **복잡도 감소** 및 **중복 제거**에 유용
- 하지만 **오버헤드를 증가시켜 성능 저하**를 유발할 수 있음
- 계층 구조는 파일 이름을 다음과 같이 변환함:
  - 파일 번호 (file number)
  - 파일 핸들 (file handle)
  - 물리적 위치
- 이 정보를 유지하기 위해 파일 제어 블록(file control block)을 사용  
  → UNIX에서는 이를 **inode**로 관리

> 각 논리 계층은 운영체제 설계자에 따라 임의의 코딩 방식으로 구현 가능

## File System Structure

### 📁 File Structure
- **논리적 저장 단위 (Logical storage unit)**
- 관련된 정보의 집합

### 💾 File System
- 보조 저장 장치(secondary storage, 예: 디스크)에 위치함
- 사용자에게 저장소 인터페이스를 제공
  - 논리 주소 → 물리 주소 매핑
- 효율적이고 편리한 데이터 저장, 검색, 접근 제공

### 🔁 디스크 특성
- **제자리 수정(in-place rewrite)** 및 **랜덤 접근(random access)** 가능
- I/O는 보통 **섹터(sector) 단위 블록**으로 처리됨
  - 일반적으로 512바이트 또는 4096바이트

### 📦 File Control Block (FCB)
- 특정 파일에 대한 **정보를 저장하는 구조체**
- 예: 파일 크기, 위치, 소유자, 접근 권한 등
### 🧩 Device Driver
- 물리 장치를 제어하는 역할을 담당

### 🧱 계층화
- 파일 시스템은 **여러 계층**으로 구성되어 있음
  - 각 계층은 기능을 분리하고 구현 독립성을 제공


### 🔄 계층 구성도

application programs
↓
logical file system
↓
file-organization module
↓
basic file system
↓
I/O control
↓
devices

### 🗂️ 주요 파일 시스템 예시

- **UNIX**: UFS (Unix File System), FFS
- **Linux**: ext3, ext4 (extended file system)
- **기타**: ZFS, GoogleFS, FUSE 등

> 하나의 운영체제 내에 여러 파일 시스템이 동시에 존재할 수 있음


## File System Operations

### 🔹 Boot Control Block
- 운영체제를 해당 볼륨에서 부팅하기 위해 필요한 정보를 포함
- 해당 볼륨에 OS가 존재할 경우에 필요
- 일반적으로 **볼륨의 첫 번째 블록**에 위치

### 🔹 Volume Control Block
- **superblock** 또는 **master file table**로도 불림
- 볼륨 자체에 대한 정보를 저장
  - 전체 블록 수
  - 사용 가능한 블록 수
  - 블록 크기
  - 자유 블록을 가리키는 포인터 또는 배열

### 🔹 Directory Structure
- 파일 시스템당 하나씩 존재하며, **파일을 구성하고 관리**
- 파일 이름과 해당 **inode 번호** (또는 MFT 항목)를 연결함


## In-Memory File System Structures

### 🗂️ Mount Table
- 시스템에 마운트된 모든 파일 시스템에 대한 정보를 저장
- 포함 내용:
  - 마운트된 파일 시스템 목록
  - 마운트 지점
  - 파일 시스템 유형 등

---

### 📋 System-wide Open-File Table
- 전체 시스템 차원의 열려 있는 파일 목록
- 각 파일에 대한 **파일 제어 블록(FCB)**의 복사본을 저장
- 파일에 대한 메타데이터 및 기타 정보 포함

### 👤 Per-Process Open-File Table
- 각 프로세스가 열고 있는 파일에 대한 정보를 저장
- 해당 프로세스만 접근 가능
- 각 항목은 **system-wide open-file table**의 항목을 가리킴
- 프로세스 고유의 접근 모드, 파일 오프셋 등도 저장

### 📌 파일 열기 (opening a file)
1. 사용자 공간에서 파일 이름 요청
2. 커널 메모리의 디렉토리 구조에서 해당 이름 탐색
3. 해당 파일의 FCB를 읽어 system-wide open-file table에 복사
4. per-process open-file table에 포인터 등록

### 📌 파일 읽기 (reading a file)
1. 사용자 공간에서 파일 디스크립터(index)로 요청
2. per-process open-file table → system-wide open-file table → FCB 참조
3. 해당 데이터 블록 읽기 수행


## File System Implementation

### 🔹 사용되는 자료구조
- 파일 시스템은 다양한 **자료구조(data structures)**를 사용하여 파일과 디렉토리를 관리함
  - 예: inode, block bitmap, file control block (FCB), directory table 등

### 🔹 데이터 및 메타데이터 조직 방식
- **데이터(data)**: 사용자가 저장한 실제 내용
- **메타데이터(metadata)**: 파일 이름, 크기, 접근 권한, 생성 시간 등
- 파일 시스템은 이를 분리하여 효율적으로 관리하고, 디스크에 저장된 구조를 통해 매핑

### 🔹 접근 방식
- 파일 시스템은 다양한 **접근 메서드**를 제공함:
  - `open()` : 파일 열기
  - `read()` : 파일 읽기
  - `write()` : 파일 쓰기
  - 기타 `close()`, `seek()` 등

> 이러한 시스템 호출은 커널이 내부 자료구조를 사용하여 물리적 저장 장치에 접근할 수 있도록 연결해 줌



## Overall Organization

- 파일 시스템 데이터 구조의 전체 조직을 구성하기 위한 기본 단계

---

### 💽 디스크를 블록 단위로 나누기

- **Block size**: 4KB
- 각 블록은 **0부터 N-1까지의 번호**를 가짐

예시:
Block 번호:
0 1 2 ... 7
8 9 ... 15
16 ... 23
24 ... 31
32 ... 39
40 ... 47
48 ... 55
56 ... 63

> 파일 시스템은 이 블록 단위 주소를 기준으로 파일, 디렉토리, 메타데이터를 배치하게 됨


## Data Region in File System

### 📦 데이터 영역 (Data Region)
- **사용자 데이터를 저장**하기 위해 디스크 일부를 데이터 블록 영역으로 예약
- 파일 시스템은 블록 단위로 데이터를 관리함
- 예시:
  - 블록 8번부터 63번까지가 Data Region
  - 각 블록은 4KB 크기
### 📌 파일 시스템이 수행해야 할 작업
- 어떤 데이터 블록이 어떤 파일에 속하는지 추적
- 각 파일의 크기, 소유자, 접근 권한 등도 관리해야 함

> 🔍 이러한 정보를 추적하기 위해 **inode(또는 FCB)** 구조가 필요함

### ❓ 다음 질문
**How do we store these inodes in the file system?**

→ inode 테이블 혹은 별도의 메타데이터 블록에 저장됨 (다음 슬라이드 주제와 연결됨)


## Inode Table in File System

### 🗃️ Inode 테이블 영역 예약
- 파일 시스템은 **inode table**을 위한 공간을 별도로 확보함
- 이 테이블은 **디스크 상의 inode 배열(array of on-disk inodes)**을 저장
### 📌 예시 구성
- **inode table 블록 범위**: 블록 3번 ~ 7번 (총 5개 블록)
- **inode 크기**: 256 bytes
- **블록 크기**: 4KB  
  → 한 블록에는 `4096 / 256 = 16`개의 inode 저장 가능
- 따라서 **총 5 블록 × 16 inode = 80 inode 저장 가능**  
  → 해당 파일 시스템에서 생성 가능한 최대 파일 수 = 80개
### 📌 정리
- inode는 **파일의 메타데이터 및 데이터 블록 포인터**를 포함
- 파일이 증가하면 inode 수요도 증가 → inode table은 파일 수 제한 결정 요인

> inode table을 통해 각 파일의 실제 데이터 블록 위치를 추적하고, 해당 파일의 속성(크기, 권한, 소유자 등)을 관리함


## Allocation Structures

### 🎯 목적
- 어떤 **inode**나 **데이터 블록**이 사용 중인지(in-use) 또는 비어 있는지(free)를 추적
- 이를 위해 **bitmap**을 사용함

### 🧮 Bitmap 구조

- **각 비트는 상태를 나타냄**:
  - `0`: free (사용되지 않음)
  - `1`: in-use (이미 할당됨)

- 주요 비트맵 종류:
  - `inode bitmap`: inode 테이블의 사용 상태 추적
  - `data bitmap`: 데이터 블록의 사용 상태 추적

- 예시 그림에서:
  - 첫 번째 inode 블록 중 일부 비트는 사용 중 (할당됨), 일부는 아직 비어 있음
  - 데이터 영역도 마찬가지로 일부만 할당됨
### ✅ 요약
- 비트맵은 파일 시스템의 공간 관리를 위한 **가볍고 효율적인 방식**
- inode와 데이터 블록을 동적으로 관리할 수 있게 함


## Super Block

### 🧱 슈퍼블록(Super Block)의 역할
- 특정 파일 시스템에 대한 **중요 메타데이터**를 저장
- OS는 파일 시스템을 마운트할 때 **슈퍼블록을 가장 먼저 읽음**

### 📌 Super Block의 정보 예시
- 전체 inode 개수
- inode 테이블의 시작 위치
- 전체 블록 수 및 블록 크기
- 데이터 영역 위치
- 비트맵 위치 등

### ✅ 요약
- 슈퍼블록은 파일 시스템의 **초기화와 전체 구조 파악에 핵심적인 블록**
- 파일 시스템이 손상되지 않도록 슈퍼블록은 종종 **백업 복사본**도 함께 저장됨


## File Organization: The inode

### 🔹 Inode Number
- 모든 파일은 고유한 **inode 번호**(inode number)로 식별됨
- 파일 시스템은 해당 번호를 바탕으로 inode가 **디스크의 어느 위치에 있는지 계산**함

---

### 📌 예시: inode number = 32

#### ① 오프셋(offset) 계산
- `32 × inode 크기(256 bytes) = 8192 bytes (= 8KB)`
- 이는 inode table 내에서의 상대 위치
#### ② inode table의 시작 주소와 합산
- inode table의 시작 주소 = 12KB  
  → (Super block: 0~4KB, inode bitmap: 4~8KB, data bitmap: 8~12KB)
- 결과적으로 inode 32는 `12KB + 8KB = 20KB` 위치에 있음

### ✅ 요약
- 파일의 inode 번호만으로 해당 inode의 정확한 디스크 위치를 계산 가능
- inode 구조는 파일 시스템 내 파일의 메타데이터와 데이터 위치를 관리하는 핵심 요소임

