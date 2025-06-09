
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




