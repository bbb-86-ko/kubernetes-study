# docker image 생성

### 기존 이미지로 컨테이너 생성
```
➜  ~ docker run -i -t --name commit_test ubuntu:14.04
root@4cda36b83f59:/# echo test_first >> first
root@4cda36b83f59:/# ^C
root@4cda36b83f59:/# exit
exit
➜  ~
```

### 생성한 컨테이너를 이미지로 만들기
```
➜  ~ docker commit \
> -a "alicek106" -m "my first commit" \
> commit_test \
> commit_test:first
sha256:76ccb98469ba2060a372506e644dc58f403567a37d3f6e79b3089bea0d02bfa1
➜  ~
```

### 생성된 이미지 확인
```
➜  ~ docker images
REPOSITORY                    TAG       IMAGE ID       CREATED         SIZE
commit_test                   first     76ccb98469ba   2 minutes ago   197MB
➜  ~
```


### commit_test:first 이미지로 컨테이너 생성
```
➜  ~ docker run -i -t --name commit_test ubuntu:14.04
root@4cda36b83f59:/# echo test_first >> first
root@4cda36b83f59:/# ^C
root@4cda36b83f59:/# exit
exit
➜  ~
```

### 생성한 컨테이너를 이미지로 만들기
```
➜  ~ docker commit \
> -a "alicek106" -m "my second commit" \
> commit_test2 \
> commit_test:second
sha256:1d295ff723d14139ca69c54837d508268e8054be303111ef109d3d7fae9755f8
➜  ~
```

### 생성된 이미지 확인
```
➜  ~ docker images
REPOSITORY                    TAG       IMAGE ID       CREATED          SIZE
commit_test                   second    1d295ff723d1   27 seconds ago   197MB
commit_test                   first     76ccb98469ba   5 minutes ago    197MB
➜  ~
```

### ubuntu:14.04 vs commit_test:first vs commit_test:second
```
docker inspect ubuntu:14.04
"Layers": [
    "sha256:f2fa9f4cf8fd0a521d40e34492b522cee3f35004047e617c75fadeb8bfd1e6b7",
    "sha256:45d8dc025525eb83384fbb6f3d49bac2e85f690a17f7fc9ab4d013e7236703bd",
    "sha256:e156c976a2ba450a30da0df824a204113c6afcf773cb09bfa186f88cd63f19dd"
]
```

```
docker inspect commit_test:first
"Layers": [
    "sha256:f2fa9f4cf8fd0a521d40e34492b522cee3f35004047e617c75fadeb8bfd1e6b7",
    "sha256:45d8dc025525eb83384fbb6f3d49bac2e85f690a17f7fc9ab4d013e7236703bd",
    "sha256:e156c976a2ba450a30da0df824a204113c6afcf773cb09bfa186f88cd63f19dd", <-- ubuntu:14.04
    "sha256:391e6f379628cdb51e6d0b455d4f16ce2ea8368b113a1dad836bcfeba8988d62"
]
```

```
docker inspect commit_test:second
"Layers": [
    "sha256:f2fa9f4cf8fd0a521d40e34492b522cee3f35004047e617c75fadeb8bfd1e6b7",
    "sha256:45d8dc025525eb83384fbb6f3d49bac2e85f690a17f7fc9ab4d013e7236703bd",
    "sha256:e156c976a2ba450a30da0df824a204113c6afcf773cb09bfa186f88cd63f19dd", <-- ubuntu:14.04
    "sha256:391e6f379628cdb51e6d0b455d4f16ce2ea8368b113a1dad836bcfeba8988d62", <-- commit_test:first
    "sha256:f13d6dede65522ea679573b43031b58db0d903b9eb627939dffd389a8ce2762d"
]
```


### 삭제해보자
- 에러가 발생했다.
```
➜  ~ docker rmi commit_test:first
Error response from daemon: conflict: unable to remove repository reference "commit_test:first" (must force) - container 4707a7104341 is using its referenced image 76ccb98469ba
```
### commit_test2를 stop 해보고 commit_test1을 삭제해보자
- 삭제가 잘된다.
```
➜  ~ docker stop commit_test2 && docker rm commit_test2
commit_test2
commit_test2
➜  ~ docker rmi commit_test:first
Untagged: commit_test:first
➜  ~
```
