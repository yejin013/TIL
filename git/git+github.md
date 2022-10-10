# git : 분산 버전 관리 시스템

- 컴퓨터 파일의 변경사항을 추적하고 여러 명의 사용자들 간에 해당 파일들의 작업을 조율하기 위함
- 데이터 무결성, 분산, 비선형 워크플로 지원
- 소스 코드가 변경된 이력을 쉽게 확인 할 수 있음 - 이러한 특징으로 인해 어느 시점부터 문제가 발생했는지 파악 가능
- 동시에 여러 버전이 있을 수 있음
- 다른 사람이 수정한 부분과 본인의 부분이 충돌했을 경우 알려주어 코드가 섞이는 것을 방지할 수 있음
- 코드에 문제가 발생했을 때 되돌릴 수도 있음
- 로컬저장소 지원으로 인한, 속도 빠름 / 자신의 로컬에 부담없이 커밋 가능 / 원격저장소가 터지더라도 로컬저장소를 통해 복구 가능
 

# github : 버전 관리 플랫폼 서비스

git을 사용하는 원격 저장소
- git 기본 명령어

- git init : git 생성하기
- git clone : repository를 로컬 저장소로 가져오기
- git branch : 브랜치 목록보기
- git branch [브랜치명] : 브랜치 생성하기
- git checkout [브랜치명] : 브랜치 선택하여 이동
- git checkout -b [브랜치명] : 브랜치 생성하고 이동
- git checkout -t [원격저장소url/브랜치명] : 원격 브랜치 선택하기
- git branch -r : 원격 브랜치 목록보기
- git branch -a : 로컬 브랜치 목록보기
- git branch -d [브랜치명 ]: 브랜치 삭제하기
- git add [파일명] : 커밋에 특정 파일 추가 (stage에 올리기)
- git add . : 커밋에 수정한 모든 파일 추가 (.gitignore에 올라가있는 파일 제외)
- git add * : 커밋에 수정한 모든 파일 추가 (.gitignore에 올라가있는 파일 포함)
- git commit : 커밋 생성
- git commit -m [메시지] : 커밋 생성 동시에 메시지 작성
- git status : 파일 상태 확인
- git push [리모트명] [브랜치명] : add하고 commit한 코드를 원격 저장소 (github) 에 보내기 (git push origin master)
- git pull : 원격 저장소에서 최신 코드 받아와 merge 하기
- git fetch : 원격 저장소에서 최신 코드 받아오기 (merge는 하지 않음)
- git merge [다른 브랜치명] : 현재 브랜치에 다른 브랜치의 수정사항을 병합
- git diff [브랜치명] [다른 브랜치명] : 변경 내용과 merge 전에 바뀐 내용 비교 가능
- git reset — hard [이동하고 싶은 커밋] : 이동하고 싶은 커밋 이전으로 이동
- git reset — soft [이동하고 싶은 커밋] : 코드는 살리고 이동하고 싶은 커밋만 취소하기
- git reset — merge : merge 취소하기
- git reset — hard HEAD && git pull : git 코드 강제로 모두 받아오기
- git config — global user.name “user_name ” : git 계정 Name 변경하기
- git config — global user.email “user_email” : git 계정 Mail 변경하기
