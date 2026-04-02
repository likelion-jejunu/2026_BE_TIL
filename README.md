# 🦁 2026 BE TIL Repository Guide 🦁

백엔드 세션 학습 내용을 기록하고 공유하기 위한 TIL 저장소입니다.
아기사자들은 아래 가이드를 따라 제출해주세요.


## 📚 목차

* [🗓 제출 기한](#-제출-기한)
* [📁 제출 방식](#-제출-방식)
* [✍️ 작성 내용 및 유의사항](#️-작성-내용-및-유의사항)
* [🚀 제출 방법 (Fork & PR)](#-제출-방법-fork--pr)
* [🔍 PR 처리 방식](#-pr-처리-방식)



## 🗓 제출 기한

* **차주 수요일 23:59까지 github 업로드 후 노션에 링크 업데이트**

  * ex) 4월 2일(목) 세션 → **4월 8일(수) 23:59까지**



## 📁 제출 방식

### 1. 레포지토리 구조

```
2026_BE_TIL/
 └── 본인 github username/
       └── 회차_주제.md
```



### 2. 폴더 및 파일 규칙

* **폴더명:** 본인 GitHub username
* **파일명:** `회차_주제.md`

    - ex) 예시

        ```
        2sseul/
        └── 1회차_Java for Spring.md
        ```


## ✍️ TIL 작성 내용/양식 및 유의사항

* TIL 작성 내용/양식 및 유의사항은 아래 노션 참고
  👉 **LikeLion at JNU 14th > TIL**


## 🚀 제출 방법 (Fork & PR)

### 1️⃣ Fork & Clone

```bash
# Fork 후
git clone https://github.com/본인아이디/2026_BE_TIL.git
cd 2026_BE_TIL
```

### 2️⃣ upstream 연결 (최초 1회)

```bash
git remote add upstream 원본레포주소(멋사TIL레포)
git remote -v
```

### 3️⃣ 브랜치 생성

```bash
git checkout -b username
```

예시

```bash
git checkout -b 2seul
```

### 4️⃣ TIL 작성 후 Commit

```bash
git add .
git commit -m "feat: n회차 TIL 제출"
```

### 5️⃣ Push

```bash
git push origin 브랜치이름
```

### 6️⃣ Pull Request 생성

* GitHub에서 **Compare & pull request 클릭**
* PR 생성
  - PR 제목: [이름] n회차 TIL 제출
    - ex) [김이슬] 1회차 TIL 제출

## 🔍 PR 처리 방식

* 제출된 PR은 **백엔드 운영진이 확인 후 merge**됩니다.
* 필요 시 수정 요청이 있을 수 있습니다.


##  🔄 (중요) 최신 상태 동기화

PR이 merge된 이후, 다음 작업(TIL 업로드) 전 반드시 진행(개인 레포지토리에서)

```bash
git checkout main
git fetch upstream
git merge upstream/main
git push origin main
```
