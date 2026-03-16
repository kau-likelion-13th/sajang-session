<aside>
🚩

멋쟁이사자처럼 13기에 오신 여러분을 환영합니다. 첫 세션은 Git Session으로 Git과 GitHub의 개념을 이해하고, Git을 활용한 버전 관리 및 GitHub을 통한 협업 방법을 실습합니다. 기본적인 명령어부터 팀 프로젝트 협업까지 단계별로 익히는 것을 목표로 합니다.

</aside>

---

## 1. Git? GitHub?

<aside>
💡

Git과 GitHub 둘의 차이가 뭘까요?

</aside>

- Git: 내 컴퓨터에서 코드 버전을 관리하는 도구
- GitHub: Git을 사용해서 인터넷에서 협업할 수 있는 플랫폼

<aside>
🎭

연극 비유로 Git과 GitHub을 이해해봅시다.

</aside>

### 1.1 Git (=연극 대본 관리 시스템)

Git은 마치 연극 대본을 관리하는 도구와 같아서,

배우들이 대본을 연습하면서 대본 내용을 바꾸거나, 원래 대본으로 되돌릴 수 있습니다.

즉, 내 컴퓨터에서 파일(코드)의 버전을 관리해주는 시스템입니다.

**✅ Git이 할 수 있는 일**

- 대본을 여러 버전으로 관리 가능
- 대본에 새로운 시나리오를 추가하고 재미가 없으면 원래대로 되돌리기
- 여러 배우(개발자)가 동시에 연습(개발) 가능

### 1.2 GitHub (=연극 대본 공유 플랫폼)

GitHub는 인터넷에 대본을 올려 배우들끼리 쉽게 공유할 수 있는 웹사이트와 같습니다.

Git을 사용해서 관리하는 대본(코드)을 온라인에 올리고, 저장하고, 다른 사람과 공유하여 협업할 수 있도록 도와줍니다.

**✅ GitHub가 할 수 있는 일**

- Git을 사용해서 만든 대본(코드)를 온라인에 저장하고 공유(원격 저장소)
- 여러 배우(개발자)들이 대본(프로젝트)을 같이 수정하고 관리
- 다른 팀원들이 쉽게 대본을 다운로드하고 변경 사항을 확인 가능
- 다른 사람의 대본(코드)를 포크(fork)해서 본인만의 스타일대로 바꿀 수 있음

### 1.3 정리

| 비교 | 연극 비유 | 실제 의미 |
| --- | --- | --- |
| Git | 대본을 관리하는 시스템 | 로컬에서 코드 버전 관리 |
| GitHub | 대본을 온라인에서 공유하는 플랫폼 | Git 저장소를 인터넷에서 관리 |

---

## 2. GitHub 사용법

### 2.1 Git 설치

Skip

### 2.2 GitHub 계정 만들기

Skip

### 2.3 GitHub에 내 프로젝트 올리기

<aside>
💡

Mac은 터미널에서, Window는 Git Bash에서 진행합니다.

</aside>

1. 내 컴퓨터에 Git 설정을 합니다.

```bash
git config --global user.name "GitHub 계정 아이디"
git config --global user.email "GitHub 계정 이메일"
```

1. 새 프로젝트 폴더를 만듭니다.

```bash
mkdir my-project  # 폴더 만들기
cd my-project  # 폴더 이동
```

1. Git 저장소를 초기화합니다.(”여기부터 버전 관리해!)
- 이제 이 폴더가 Git이 관리 하는 저장소가 됩니다.

```bash
git init
```

1. GitHub에 새 저장소(repository)를 만듭시다.
- GitHub에 로그인 후 우측 상단 + 버튼 클릭하여 “New repository” 선택
- Repository Name에 “멋사닉네임-project” 입력
- Public 또는 Private 선택
- Create repository 버튼 클릭
1. 프로젝트를 GitHub 저장소에 연결하기
- GitHub에서 제공하는 repository URL을 복사
- 터미널/Git Bash에서 연결

```bash
git remote add origin repositoryURL
```

1. 그 전에 Git Bash에서 master 브랜치를 main 브랜치로 변경합니다.

```bash
git branch -M main
```

1. GitHub에 파일을 올려봅시다.
- 터미널에서 내 프로젝트로 이동
- pull 받기

```bash
git pull origin main
```

- 아래 명령어를 입력하여 myfile.txt 파일 생성

```bash
touch myfile.txt
```

- 파일 추가하기(모든 파일을 Git에 추가하는 명령어임)

```bash
git add .
```

- 커밋하기 (변경사항 저장하기)
- -m “메시지” → 커밋 메시지(변경 내용 설명)를 작성해야 함

```bash
git commit -m "initial commit"
```

- GitHub에 올리기

```bash
git push origin main 
```

- GitHub에 myfile.txt 파일이 올라간 것을 확인할 수 있음

### 2.4 주요 Git 명령어 정리

| 명령어 | 설명 |
| --- | --- |
| `git init` | Git 저장소 초기화 |
| `git status` | 현재 상태 확인 |
| `git add .` | 모든 변경사항 추가 |
| `git commit -m "메시지"` | 변경사항 저장 |
| `git push origin main` | GitHub에 업로드 |
| `git pull origin main` | GitHub에서 최신 코드 가져오기 |
| `git log` | 커밋 내역 보기 |
| `git checkout -b 새브랜치` | 새 브랜치 만들기 |
| `git merge 브랜치명` | 브랜치 합치기 |

### 2.5 VSCode와 GitHub 연동

1. VSCode를 엽니다.
2. Ctrl + Shift + P (맥은 Cmd + Shift + P) → Git: Clone 입력 후 선택
3. GitHub URL 입력
4. 폴더 선택 → 자동으로 코드가 다운로드 됨
5. Ctrl + Shift + G
6. Sign in to GitHub 클릭 후 로그인

---

## 3. 실습

<aside>
💡

이제 GitHub 사용법을 알았으니 2명이 한 팀이 되어 간단한 팀 프로젝트를 진행해봅시다.

</aside>

- 각자의 역할이 다르지만, 함께 과정을 따라가 보세요.

### 3.1 A의 첫 번째 할 일

자신의 GitHub 저장소를 Clone 해둡니다.

```bash
git clone repositoryURL
cd 저장소 이름 # 저장소 폴더로 이동
```

B를 GitHub 저장소에 초대합니다.

- GitHub에서 저장소로 이동
- Settings 클릭
- 왼쪽 메뉴에서 Collaborators 선택
- Add people 클릭 후 B의 GitHub 아이디 입력 → 초대
- B가 GitHub에서 초대 수락
- 이제 B도 A의 저장소에서 직접 수정 및 푸시가 가능함

### 3.2 B의 첫 번째 할 일

A의 저장소를 자신의 컴퓨터로 Clone합니다.

우선 새 폴더를 생성합니다.

```bash
mkdir project1
cd project1
```

- A의 GitHub 저장소에서 Code 버튼 클릭
- HTTPS URL 복사
- 터미널/Git Bash에서 아래 명령어 입력

```bash
git clone repositoryURL
cd 저장소 이름 # 저장소 폴더로 이동
```

- 이제 B는 A의 저장소를 자신의 컴퓨터에 복사 완료함

### 3.3 B의 두 번째 할 일

자신의 닉네임을 브랜치 이름으로 사용하여 새로운 브랜치를 생성하고, 해당 브랜치로 이동합니다.

```bash
git checkout -b nickname
```

myfile.txt 파일을 수정하고 푸시합니다.

- myfile.txt 파일을 열어 “Hi, I’m nickname.” 작성 또는 아래 명령어 입력

```bash
echo "Hi, I'm nickname" >> myfile1.txt
```

- 아래 명령어를 입력하여 Git에 추가하고 커밋, 푸시

```bash
git add .
git commit -m "nickname add message"
git push origin nickname
```

GitHub에서 Pull Request(PR)를 생성합니다.

- GitHub 저장소로 이동하여 Pull Request 탭으로 들어가 New Pull Request 버튼 클릭
- nickname 브랜치를 main 브랜치에 병합하는 PR을 생성
- PR 제목: “Add message” / PR 설명: “This PR adds a message to myfile.txt”
- Create Pull Request 버튼 클릭

### 3.4 A의 두 번째 할 일

B의 PR을 승인하고 병합합니다.

- Merge pull request 버튼을 눌러 main 브랜치에 병합
- GitHub 확인

로컬 main 브랜치를 업데이트합니다.(A의 변경 사항 가져오기)

- PR 승인 후, 로컬 main 브랜치를 최신 상태로 업데이트 하기 위해 다음과 같은 명령어 입력

```bash
git checkout main
git pull origin main
```

자신의 닉네임을 브랜치 이름으로 사용하여 새로운 브랜치를 생성하고, 해당 브랜치로 이동합니다.

```bash
git checkout -b nickname
```

myfile.txt 파일을 B와 같이 수정하고 푸시합니다.

- myfile.txt 파일을 열어 “Hi, I’m nickname.” 작성 또는 아래 명령어 입력

```bash
echo "Hi, I'm nickname" >> myfile2.txt
```

- 아래 명령어를 입력하여 저장소에 푸시

```bash
git add .
git commit -m "nickname add message"
git push origin nickname
```

GitHub에서 Pull Request(PR)를 생성합니다.

- GitHub 저장소로 이동하여 Pull Request 탭으로 들어가 New Pull Request 버튼 클릭
- nickname 브랜치를 main 브랜치에 병합하는 PR을 생성
- PR 제목: “Add message” / PR 설명: “This PR adds a message to myfile.txt”
- Create Pull Request 버튼 클릭

### 3.5 B의 마지막 할 일

A의 PR을 승인하고 병합합니다.

- Merge pull request 버튼을 눌러 main 브랜치에 병합
- GitHub 확인

로컬 main 브랜치를 업데이트합니다.(A의 변경 사항 가져오기)

- PR 승인 후, 로컬 main 브랜치를 최신 상태로 업데이트 하기 위해 다음과 같은 명령어 입력

```bash
git checkout main
git pull origin main
```

### 3.6 사용한 브랜치 삭제

서로의 병합이 모두 끝났으니, A와 B 모두 자신이 사용한 브랜치를 삭제합니다.

### 3.7 GitHub 저장소 삭제

<aside>
💡

이제 팀 프로젝트에서 기본적인 Git 협업은 가능해졌으니, GitHub 저장소를 삭제해봅시다.

</aside>

- GitHub 저장소는 한 번 삭제하면 복구할 수 없습니다.
- 소유자만 저장소 삭제가 가능합니다.(초대된 팀원은 삭제 불가)

1. GitHub에서 저장소를 삭제합니다.
- Settings 클릭
- 왼쪽 메뉴에서 General 클릭
- 아래로 스크롤하여 Danger Zone 찾기
- Delete this repository 버튼 클릭
- “저장소 이름” 입력 후 삭제 확인

1. 내 컴퓨터에서도 저장소를 삭제합니다.
- 터미널/Git Bash를 열고, 저장소가 있는 폴더로 이동하여 아래 명령어를 입력

```bash
rm -rf 저장소이름
```

---

## 4. GitHub Session 과제

운영진이 13기 아기 사자 여러분들을 모두 **Likelion-13th 조직**에 초대하였습니다.

1. 앞으로 1년 동안 진행할 프로젝트를 위해 **Likelion-13th 조직 내에서** “Nickname-Frontend”, “Nickname-Backend” 이렇게 **2개의 Repository**를 생성해주세요.
2. “nickname-frontend” repository 를 **VSCode에 Git Clone** 해주세요.
3. “nickname-frontend” repository에 nickname.txt 파일을 추가한 후 **add, commit, push**까지 해주세요.

(우리가 오늘 배운 명령어를 사용하지 않고 쉽게 할 수 있습니다. **→** 구글링을 통해 알아보세요.)

1. 2개의 repository HTTPS URL을 복사하여 GitHub Session 과제 제출 페이지에 작성해주세요.

과제 기한은 **3월 18일 화요일 23시까지**입니다.

과제를 수행하며 궁금한 사항이 있다면, 단톡방 혹은 개인 톡으로 질문 주시면 됩니다.

파이팅 :)
