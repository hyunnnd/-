
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



