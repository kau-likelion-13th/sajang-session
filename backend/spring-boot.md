
💡맥북은 인텔리제이 프로가 아니면 데이터베이스가 안된다고 합니다.

<br>

## 1️⃣프로젝트 생성

[**start.spring.io](http://start.spring.io)   👈**

![image.png](attachment:d1a1dc92-2f81-4ab7-abac-2ed82e505289:image.png)

![image.png](attachment:466fdf4d-e893-48a0-a177-9f8ccfb0cc73:1ed05782-8948-4ebf-bb30-11aa21a10198.png)

**Project Metadata**

- Group : 기업 도메인명 
→ likelion13th 으로 설정해주겠습니다!
- Artifact : 결과물
→ 원하는 쇼핑몰 이름으로 해주세요~
- Name : 프로젝트명 
(일반적으로 Artifact와 동일)
- Description : 설명
- Package name : 패키지 이름
(자동으로 설정해줌)

![image.png](attachment:6a88d06c-0e12-431b-90db-4195589cfb9e:image.png)

**Dependencies** 

- Spring Web

<aside>
💡

이때 자바 설정은 **Java17** 로 진행합니다!

→ 윈도우와 맥북 자바 설치 방법이 다릅니다. 이 부분은 인터넷을 참고해주시길 바랍니다!

</aside>

### 📌 방금 한 Dependencies 추가란..?

→ 어떤 기능을 사용할지 선택하는 과정입니다!

→ 하나하나 라이브러리를 찾아 추가할 필요 없이 자동으로 추가 해줌

- Spring Web을 추가해줌으로써 웹을 개발할 때 필요한 라이브러리를 자동으로 추가해 줌
- 만약 intelliJ 한 프로젝트에서 프론트엔드까지 다룬다면..?
→ **Thymeleaf**를 추가하면 HTML을 동적으로 렌더링 할 수 있는 템플릿 엔진이 자동으로 설정됨 
→ Spring Boot와 함께 사용하면, HTML 코드에 데이터를 동적으로 넣을 수 있음
- (DI는 객체끼리 연결해주는 것, build.gradle의 dependencies 추가는 프로젝트에 필요한 라이브러리를 추가하는 것!!!)

- +  동적으로 렌더링..?
    
    ![스크린샷 2025-02-10 124647.png](attachment:1fa8aed8-cad2-4f1d-934b-a07510da56af:스크린샷_2025-02-10_124647.png)
    
    먼저 **정적 렌더링**은 단어 의미 그대로 ‘변화가 없는’ 방식입니다. 미리 만들어진 HTML을 그대로 전달하는 방식이라, 항상 같은 내용이 표시돼요.
    
    하지만 사용자에 따라 다른 데이터가 보여야 할 페이지들이 있잖아요?! 예를 들어, 마이페이지처럼 사용자별로 맞춤형 정보를 제공해야 하는 경우  HTML로 미리 만들어두기엔 한계가 있습니다.
    
    이럴 때 필요한 것이 바로 **동적 렌더링**을 해주어야 합니다! 이 방식은 사용자의 요청에 따라 서버에서 데이터를 가공해 HTML을 동적으로 생성하는 방식입니다.
    

---

### **📂 이제  GENERATE 눌러서 파일을 다운로드해주세요!**

- 로컬디스크 (C:)→ 사용자 → (사용자 이름) → Documents 에서 압축을 풀어줍시다!
    
    D 드라이브, 네트워크 드라이브, 특정 사용자 폴더에 있으면 IntelliJ가 정상적으로 접근하지 못할 수 있어요.
    

---

![스크린샷 2025-02-02 221902.png](attachment:804583d1-f38a-4b09-a337-30a185bb3aee:스크린샷_2025-02-02_221902.png)

**IntelliJ 에 들어가서 → 파일 →  열기**

→ 압축 푼 경로로 들어가서 열어줍니다!

파일 이름 안에 파일이름이 있으면 걜!!! 열어줘야해요!!!!!

또는 **압축을 푼 파일에서 우클릭 후**

**Open Folder as IntelliJ~~ 클릭**

프로젝트 신뢰

---

⚠️ Dependencies 추가 시에 **맥북**과 같은 경우는 …

위 사이트에서 체크를 한 후, 굳이 다운을 받을 필요 없이 build.gradle 파일에 들어가면 의존성이 추가가 된 경우가 있습니다. 이런 경우는 그대로 진행하시면 됩니다!

---

로딩이 조금 걸립니다!

필요한 라이브러리와 아까 우리가 넣어준 의존성을 다운로드하고 설정하는 중이에요!

---

### ⚙️ 잘 설정되었는지 확인해볼게요!

1. 프로젝트명(이하 shop..) 한 번 눌러서 아래에 코끼리 모양 **build.gradle**에서 아까 넣었던 **dependencies** 애들 있는지 확인

![스크린샷 2025-02-10 115643.png](attachment:79297415-b248-4f9e-8c54-6a7f879138c9:스크린샷_2025-02-10_115643.png)

→ 그럴 리는 거의 없지만 호옥시라도 없다면 직접 추가해 줘도 됩니다.

1. **src→main→java→likelion13th.shop→ShopApplication 실행**

![스크린샷 2025-02-10 120532.png](attachment:bc1b3d74-7777-4c2a-8399-8d82dbd74b3c:스크린샷_2025-02-10_120532.png)

- 실행하면 밑에 쭈욱 뜰거에요.

![화면 캡처 2025-02-10 120803.png](attachment:c43ea9d0-58e8-41fc-a5e5-e406feba5527:화면_캡처_2025-02-10_120803.png)

→ 웹 서버가 8080 포트에서 실행되었다는 뜻

- (실행한 상태에서!!) 주소창에 [localhost:8080](http://localhost:8080) 을 쳐줍니다.

![스크린샷 2025-02-10 121829.png](attachment:87720ebd-f131-42dc-a770-12c4d8b61d45:ce626414-3da4-497e-84c8-65b49227849f.png)

아무것도 안 적어주었기 때문에 이런 에러가 뜨는 게 정상입니다!

1. 설정 → 프로젝트 구조 → 프로젝트 → **SDK 설정에 17**로 되어있는지 확인!
* JDK는 Java Development Kit를 의미합니다.

---

이제 GitHub와 이 프로젝트를 연결하러 가봅시당

## 2️⃣ GitHub 연결

- Window는 파일 우클릭 후  Git Bash 열기
- Mac은 터미널에서…

![스크린샷 2025-02-10 130126.png](attachment:7a9743ed-f6e2-4b13-a998-5921ecd36731:스크린샷_2025-02-10_130126.png)

1. 초기 설정을 해줄게요.

```bash
git config --global user.name "GitHub 계정 아이디"
git config --global user.email "GitHub 계정 이메일"
```

1. 현재 폴더를 Git 저장소로 만들기

```bash
 git init
```

1. 과제로 만들었던 레포지토리를 열어주세요!

![README 추가를 안하면 주소가 안보일 수 있다고 하네요](attachment:fdc3039e-1236-4b1d-bcef-87b86fa4088d:스크린샷_2025-02-10_130837.png)

README 추가를 안하면 주소가 안보일 수 있다고 하네요

- 주소 복사! → 다시 Git Bash로
1. 로컬 저장소와 GitHub 원격 저장소 연결

```bash
git remote add origin <주소>
```

> 이때 ‘로컬 저장소’는 내 컴퓨터에서 작업하고 있는 Git 저장소를 의미해요. ‘origin’은 원격 저장소를 뜻한다고 보면 돼요~
> 

(git branch -M main) // 현재 브랜치 이름을 main으로 변경

(git pull origin main)

1. 모든 파일 Git에 추가하기

```bash
git add .
```

1. 커밋하여 변경 사항을 저장하기

```bash
 git commit -m “message” 
 📌 커밋 메시지는 현재 변경 내용을 설명하는 메시지!
```

1. 이제 원격 저장소 = GitHub에 올리기

```bash
//첫 푸시는 
 git push -u origin main
 //main은 브랜치 이름
```

→ 다음 푸시부터는 git push 만 입력해도 자동으로 같은 브랜치에 푸시돼요

🫏🫏🫏깃허브 레포지토리로 가서 확인해보아요~!!

📌 **기억해놓으면 좋을 푸시 순서**

1️⃣ git add .

2️⃣ git commit -m “message’

3️⃣ git push origin <branch> / git push
