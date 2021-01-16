# docker image 추출 로드

### 추출
- save 로 추출한다.
- -o 옵션은 추출될 파일명을 입력한다.
```
➜  ~ docker save -o ubuntu_14_04.tar ubuntu:14.04
➜  ~ ls
...<생략>...
ubuntu_14_04.tar
```

### 로드
- load 로 로드한다.
```
➜  ~ docker load -i ubuntu_14_04.tar
Loaded image: ubuntu:14.04
➜  ~
```

### export & import
- export : 명령어는 컨테이너의 파일시스템을 tar 파일로 추출하며 컨테이너 및 이미지에 대한 설정정보를 저장하지 않는다.
- image : 추출된 tar를 이용해요 이미지로 다시 저장한다.
```
docker export -o rootFS.tar mycontainer
docker import rootFS.tar mycontainer:0.0
```
