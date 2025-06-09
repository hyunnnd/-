
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


