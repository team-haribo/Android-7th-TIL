Git Commit
-------------      
커밋은 로컬 저장소(컴퓨터에 저장된 폴더)에 있는 것을 임시 저장, 복사본 역할을 한다.
```
    git add .
```

위처럼 디렉토리의 모든 파일을 저장한 후
```
    git commit -m "커밋 메세지" 
```
위처럼 커밋을 해준다.

Git Push
-------------   
푸쉬는 로컬 저장소에 있는 커밋 파일을 원격 저장소(온라인에 저장된 프로젝트 폴더)에 업로드한다.(저장할 브랜치를 지정할 수 있다.)
```
    git push <원격 저장소> <브랜치>   
```
위처럼 푸쉬를 해준다.

git Flow
-------------   
git flow란 협업하여 개발할 때 쓰이는 방법이다.
#### git flow의 branch
- (main) master   
  제품을 배포하는 브랜치
- (main) develop   
  개발 브랜치이자 각자 작업한 단위 기능을 머지(merge) 한 브랜치
- feature   
  **단위 기능** 브랜치, 기능이 완성되면 develop 브랜치에 머지(merge)함
- release   
  master 브랜치에 보내기 전 **제품을 검사** 하는 브랜치
- hotfix   
  배포한 후 버그가 생겼을 때 **긴급 수정** 하는 브랜치

1. master branch로 시작한다.
2. 똑같은 branch를 develop branch로 생성한다.
3. 단위 기능을 구현할 때 develop branch에서 feature branch를 생성 후 구현한다.
4. 단위 기능을 검토했다면 develop branch에 머지(merge)한다.
5. 모든 기능을 구현했다면 develop branch를 release branch로 만들어 수정, 보완을 한다.
6. 검토가 끝났으면 master branch와 develop branch로 보낸다. (master branch에선 태그를 생성 후 배포한다.)
7. 배포 후 버그가 있을 경우 hotfixes branch를 만들어 수정 후 태그를 생성하고 수정 배포한다.   
