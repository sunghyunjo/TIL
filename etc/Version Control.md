# Version Control (Git)
  
## Git의 동작 방식
Git이외의 버전 관리 시스템을 사용해보지 않았기 때문에, 다른 VCS와의 비교보다는 Git자체의 동작 원리에 대해 정리해보았다.

### Snapshot방식
- Git의 데이터는 파일 시스템의 스냅샷이라고 할 수 있다.
- Git은 commit하거나 프로젝트의 상태를 저장할 때마다, 파일이 존재하는 그 순간을 중요하게 여긴다. 
- 파일이 달라지지 않았다면 Git은 성능을 위해 파일을 저장하지 않는다.
![Git의 snapshot저장 방식](https://git-scm.com/figures/18333fig0105-tn.png)

### 로컬에서 명령 실행
Git은 프로젝트의 모든 히스토리를 로컬 디스크에 저장한다. 이로 인해 얻을 수 있는 장점들이 있다.  

- 오프라인 상태에서 예전 버전과 최신 버전을 빠르게 비교할 수 있다. 
- 오프라인 상태에서 커밋이 가능하다.

### CheckSum
- Git은 모든 데이터를 저장하기 전에 체크섬(or Hash)를 구하고, 그 체크섬으로 데이터를 관리한다. 
- 체크섬 없이 어떠한 파일이나 디렉토리를 변경할 수 없다.
- Git은 파일의 이름으로 저장하지 않고, 해당 파일의 해시로 저장한다.

      
     
       
## Git의 상태관리
Git은 파일을 세 가지 상태로 관리한다.

- Committed : 데이터가 로컬DB에 안전하게 저장된 상태.
- Modified : 수정한 파일을 아직 로컬DB에 커밋하지 않은 상태.
- Staged : 현재 수정한 파일을 곧 커밋할 것이라고 표현한 상태.

위의 세 가지 상태는 Git 프로젝트의 세 가지 단계와 연결돼 있다.

![워킹 디렉토리, Staging Area, Git 디렉토리](https://git-scm.com/figures/18333fig0106-tn.png)

### Git Directory
- Git이 프로젝트의 메타데이터와 객체 데이터베이스를 저장하는 곳
- 다른 컴퓨터에 있는 저장소를 Clone할 때, Git디렉토리가 생성된다.

### Working Directory
- 프로젝트의 특정 버전을 Checkout한 것.
- 지금 작업하는 디스크에 있는 Git 디렉토리에 압축된 DB에서 파일을 가져와 워킹 디렉토리를 생성한다.

### Staging Area
- Git Directory안에 있다.
- 단순한 파일이고, 곧 커밋할파일에 대한 정보를 저장.

### 정리하자면 
Git이 기본적으로 하는 일은,

1. 워킹 딜렉토리에서 파일을 수정한다.
2. Staging Area에 파일을 Stage해서 커밋할 스냅샷을 생성한다.
3. Staging Area에 있는 파일들을 커밋해서 Git디렉토링 영구적인 스냅샷으로 저장한다.

### 상태에 따라 분류
- Committed 상태 : Git 디렉토리에 있는 파일들.
- Staged 상태 : 파일을 수정하고 Staging Area에 추가된 파일들.


## Git 사용하기
기본적인 git 셋팅과정보다, 내부에 사용되는 파일이나 설정 정보에 대해 자세히 알아보았다.
### git config
git에 대한 설정을 할 때, `git config`라는 것을 사용해 보았을 것이다. 
git은 이 설정에 따라 동작한다.
이 때 사용하는 파일은 3가지가 있다.

- `/etc/gitconfig`파일 : 시스템의 모든 사용자와 모든 저장소에 적용되는 설정. `git config system`옵션으로 이 파일을 읽고 쓸 수 있다.
- `~/.gitconfig`파일 : 특정 사용자에게만 적용되는 설정이다. `git config --global`옵션으로 이 파일을 읽고 쓸 수 있다.
- `.git/config`파일 : 이 파일은 git디렉토리에 있고, 특정 저장소(혹은 현재 작업 중인 프로젝트)에만 적용된다. 각 설정은 역순으로 우선시 된다. 그래서 `.git/config`가 `/etc/gitconfig`보다 우선한다.

### File Lifecycle
워킹 디렉토리의 모든 파일은 크게 **Tracked(관리 대상)**와 **Untracked(관리대상이 아님)**로 나눈다.

- **Tracked파일** : 이미 스냅샷에 포함돼 있던 파일
	- Tracked파일은 Unmodified(수정하지 않음), Modified(수정함) 그리고 Staged(커밋하면 저장소에 기록)상태 중 하나를 갖는다.

- **Untracked파일** : Tracked파일이 아닌 모든 것.

처음 저장소를 clone하면 모든 파일은 Tracked이면서 Unmodified상태가 된다. 왜냐면, 파일을 Checkout하고 나서 아무것도 수정하지 않았기 때문이다.

![File Status Lifecycle](https://git-scm.com/figures/18333fig0201-tn.png)

### 리모트 저장소
인터넷이나 네트워크 어딘가에 있는 저장소를 말한다.



## Git 브랜치
커밋 사이를 가볍게 이동할 수 있는 어떤 포인터 같은 것.

- 기본적으로 Git은 `master`브랜치를 만든다.
- 최초로 커밋 시, Git은 master라는 이름의 브랜치를 만들어 자동으로 가장 마지막 커밋을 가리키게 한다.

![가장 최근 커밋 정보를 가리키는 브랜치](https://git-scm.com/figures/18333fig0303-tn.png)

깃 브랜치를 하나 더 생성하면, 어떨까?

`$ git branch testing`

아래 그림과 같이 새로 만든 브랜치도 지금 작업하고 있던 마지막 커밋을 가리킨다.
![커밋 개체를 가리키는 두 브랜치](https://git-scm.com/figures/18333fig0304-tn.png)

## HEAD
- 지금 작업 중인 브랜치가 무엇인지 파악하는 특수한 포인터
- 이 포인터는 지금 작업하는 로컬 브랜치를 가리킨다.

![현재 브랜치를 가리키는 HEAD포인터](https://git-scm.com/figures/18333fig0306-tn.png)

만일 이때, 새로운 커밋을 해본다면 결과는 아래와 같이 바뀐다.

![커밋으로 바뀐 HEAD](https://git-scm.com/figures/18333fig0307-tn.png)

하지만, 여기서 새로 커밋으로 인해 testing브랜치는 앞으로 이동했지만, `master`브랜치는 여전히 이전 커밋을 가리킨다.

이때 `$ git checkout master`을 통해 `master`브랜치로 이동하면 결과는 아래와 같다.

![HEAD가 master로 이동함](https://git-scm.com/figures/18333fig0308-tn.png) 

여기서 master브랜치에 새로운 커밋을 진행하여도, 프로젝트 히스토리는 아래와 같이 testing브랜치와 별개로 존재한다.

![브랜치 히스토리가 서로 독립적](https://git-scm.com/figures/18333fig0309-tn.png)

[출처 및 참고 : Git 공식 문서](https://git-scm.com/book/ko/v1/%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0)
