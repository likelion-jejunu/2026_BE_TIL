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


## ✍️ 작성 내용 및 유의사항

* 작성 내용 및 유의사항은 아래 노션 참고
  👉 **LikeLion at JNU 14th > TIL**




## 🚀 제출 방법 (Fork & PR)

### 1️⃣ Fork

1. 본 레포지토리 (`2026_BE_TIL`) 접속
2. 우측 상단 **Fork 클릭**


### 2️⃣ Clone

```bash
git clone https://github.com/본인아이디/2026_BE_TIL.git
cd 2026_BE_TIL
```

### 3️⃣ 파일 생성 및 TIL 작성

```bash
회차_주제.md
```


### 4️⃣ Commit & Push

```bash
git add .
git commit -m "feat: [username] n회차 TIL 제출"
git push origin main
```

### 5️⃣ Pull Request 생성

1. GitHub에서 본인 레포 접속
2. **"Compare & pull request" 클릭**
3. PR 생성
    - PR 작성시 제목은 `[이름] n회차 TIL 제출`로 작성해주세요.
    - ex) `[김이슬] 1회차 TIL 제출`

## 🔍 PR 처리 방식

* 제출된 PR은 **백엔드 운영진이 확인 후 merge**됩니다.
* 필요 시 수정 요청이 있을 수 있습니다.
