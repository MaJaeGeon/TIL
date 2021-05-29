# Docker

### Docker 설치
```shell
curl -s https://get.docker.com | sudo sh
```

### 이미지 가져오기
```shell
docker pull [image repository name]
```

### 설치된 이미지 목록보기
```shell
docker images
```

### 이미지 삭제 
```shell
docker rmi [image id]
```

### 실행중인 컨테이너 보기
```shell
docker ps
```

### 모든 컨테이너 보기
```shell
docker ps -a
```

### 컨테이너 생성
```shell
docker [image name]
```

### 컨테이너 실행
```shell
docker start [container id]
```

### 컨테이너 생성과 실행 한번에 하기
```shell
docker run [image]
```

### 컨테이너 접속
```shell
docker attach [container id]
```

### 컨테이너 종료
```shell
docker stop [container id]
```

### 컨테이너 삭제
```shell
docker rm [container id]
```