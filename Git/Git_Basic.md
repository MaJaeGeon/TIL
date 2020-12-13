# Git Basic

## 1. Git 개요
Git 은 `분산 버전 관리 시스템 (DVCS)` 로 크게 버전관리, 백업, 협업을 할 수 있는 기능을 제공해준다.

- 버전관리
문서를 수정할 때 언제, 어디를 수정하였는지 구체적으로 기록할 수 있게 해준다.
- 백업
유실될 수 있는 자료를 클라우드와 같은 원격 저장소에 저장할 수 있게 해준다.

- 협업
원격저장소를 사용하여 여러사람들과 온라인으로 협업할 수 있게 해준다.


## 2. Git 주요 명령어

#### 1. Git 저장소 생성 및 초기화
특정 디렉토리의 파일들을 Git 에 의해 버전관리 될 수 있게 해주며 이 디렉토리를 `작업 디렉토리 (working directory)` 또는 `작업 트리 (working tree)` 라고 부른다.
이 명령어를 사용하면 디렉토리안에 .git 이라는 숨겨진 디렉토리가 생생된다.
```shell 
$ git init 
```

#### 2. 스테이지 등록
작업 디렉토리안의 파일들은 기본적으로 git 에 의해 관리되지않는 `untracked` 상태이기 때문에 이 명령어를 통해 파일들이 git 에게 관리될 수 있도록 상태를 `tracked`로 변경 해준다.
이 명령어가 적용된 파일은 스테이지 (Stage) 라는 가상의 공간에 등록되며 git 에게 커밋대기 상태임을 알려준다.
```shell 
$ git add 파일명
```

#### 3. 상태확인
워킹디렉토리안의 파일들이 관리되고있는지, 커밋대기상태인지 와 같은 현재상태를 알려준다.
```shell 
$ git status 
```

#### 4. 버전 생성
스테이지에 등록되어있는 파일들을 버전으로 저장하며 커밋 내용에는 어떤 변화가 있었는지 기록한다.
```shell 
$ git commit -m '커밋 내용'
```

#### 5. 커밋 기록 확인
커밋을 통해 생성된 버전들을 볼 수 있다.
```shell 
$ git log
```