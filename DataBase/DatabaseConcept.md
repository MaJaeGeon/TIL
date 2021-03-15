# 데이터베이스
데이터가 모이면 정보, 정보가 모이면 지식이된다.  
데이터는 관찰의 결과로 나타난 정량적 혹은 정성적인 실제 값을 말하고,  
정보는 데이터에 의미를 부여한것을 말하며, 지식은 사물이나 현상에 대한 이해를 말한다.  

데이터베이스는 조직에 필요한 정보를 얻기 우해 논리적으로 연관된 데이터를  
모아 구조적으로 통합해놓은것을 의미한다.

# 데이터베이스의 개념

- 통합된 데이터 : 여러 데이터들에서 중복을 최소화하여 하나로 저장한 데이터를 의미
- 저장된 데이터 : 문서로 보관된 데이터가 아닌 컴퓨터 장치에 저장된 데이터를 의미
- 운영 데이터   : 임시로 저장된 데이터가 아닌 조직의 목적을 위해 사용되는 데이터를 의미
- 공용 데이터   : 한 사람 또는 한 업무에서만 사용되는것이 아닌 공동으로 사용되는 데이터를 의미


# 데이터베이스의 특징

- 실시간 접근성     : 데이터베이스는 사용자의 요청에 실시간으로 결과를 서비스한다.
- 계속적인 변화     : 데이터베이스에 저장된 데이터는 삽입, 삭제, 수정 등의 작업으로 바뀐다.
- 동시 공유         : 데이터베이스는 서로 다른 업무 또는 여러 사용자에게 동시에 공유된다.
- 내용에 따른 참조  : 데이터베이스에 저장된 데이터는 데이터의 값에 따라 참조된다.

# 정보 시스템의 발전

- 파일 시스템 : 파일 시스템은 데이터를 파일단위로 파일 서버에 저장한다.
  각 응용 프로그램이 독립적으로 파일을 다루기 때문에 데이터가 중복되거나 일관성이 훼손될 수 있다.

- 데이터베이스 시스템 : DBMS를 도입하여 데이터를 통합관리하는 시스템으로,  
  DBMS가 설치되어 데이터를 가진쪽을 서버, 외부에서 데이터를 요청하는쪽을 클라이언트라고 한다.  
  데이터를 저장하기 전 설계과정을 거쳐 데이터의 중복을 줄이고 데이터를 표준화하여 무결성을 유지한다.

- 웹 데이터베이스 시스템 : 데이터베이스를 웹 브라우저에서 사용할 수 있도록 서비스하는 시스템이다.
  웹 브라우저를 통해 웹 서버에 접속하여 데이터를 요청하며 웹 서버는 DBMS 서버에 요청을 전달한다.

- 분산 데이터베이스 시스템 : 데이터가 발생하는곳이 여러곳이거나  
  여러곳에 분산된 데이터베이스의 상호 연동이 필요한 경우 DBMS 서버를 분산 및 연결하여 운영하는 시스템이다.

# 파일 시스템과 DBMS

### 파일 시스템과 DBMS의 비교
구분 | 파일 시스템 | DBMS 
:---|:---|:---
데이터 정의 및 저장 | 정의 : 응용 프로그램 / 저장 : 파일 시스템 | 정의 : DBMS / 저장 : 데이터베이스
데이터 접근 방법 | 응용프로그램이 파일에 직접 접근 | 응용 프로그램이 DBMS에 파일 접근을 요청
사용 언어 | C, C++, C# 등 | C, C++, C# 등과 SQL 
CPU/주기억장치 사용 | 적음 | 많음

### DBMS의 장점
구분 | 파일 시스템 | DBMS 
:---|:---|:---
데이터 중복 | 데이터를 파일 단위로 저장하므로 중복 가능성 높음 | DBMS를 이용하여 데이터를 공유하기 때문에 중복 가능성 낮음
데이터 일관성 | 데이터의 중복 저장으로 일관성이 결여됨 | 중복 제거로 데이터의 일관성이 유지됨
데이터 독립성 | 데이터 정의와 프로그램의 독립성 유지 불가능 | 데이터 정의와 프로그램의 독립성 유지 가능
관리 기능 | 보통 | 데이터 복구, 보안, 동시성 제어, 데이터 관리 기능들을 수행
프로그램 개발 생산성 | 나쁨 | 짧은 시간에 큰 프로그램을 개발할 수 있음
기타 장점 | 별도의 소프트웨어 설치가 필요 없음 | 데이터 무결성 유지, 데이터 표준 준수 용이 