
![스크린샷 2024-05-31 오후 3 00 03](https://github.com/hari1010haley/ZeroSugarProject/assets/153698072/cdcfe73e-5bd3-46fe-a162-4a20b395e6d9)

<# Member Management System>  
재등록 예정 회원을 효율적으로 관리하기 위한 관리자 웹 시스템


<i>주요 기능</i> 
<재등록멤버확인 가능><br>
1. 현재날짜 기준 만료일이 15일 전후안에 있는 경우 재등록멤버 버튼에서 list로 확인 가능
2. 서비스측면에서 더 정확한 재등록 관리를 위해 재등록여부, 재등록 횟수 Colomn 추가<br>

---

## 📌 Problem

필라테스 센터 관리자 고객의 needs에서 회원의 만료일을 수기로 관리해야 하는 비효율이 있었습니다.  
특히 재등록 대상 회원을 매번 확인해야 하는 번거로움과 관리 누락 문제가 발생했습니다.  

이를 해결하기 위해,  
회원 정보 등록 및 만료일 기반 재등록 대상 자동 분류 기능을 갖춘 관리자 웹 시스템을 개발했습니다.

---

## 🧠 Design Decisions

### 1. remDays를 저장하지 않고 계산
만료일까지 남은 일수는 시간에 따라 변하는 값이기 때문에 DB에 저장할 경우 정합성 문제가 발생할 수 있습니다.  
따라서 `LocalDate`와 `endDate`를 비교하여 조회 시점에 동적으로 계산하도록 설계했습니다.

---

### 2. 입력 폼 최소화
운영 목적이 “재등록 대상 관리”에 있었기 때문에  
성별, 주소 등의 정보는 제거하고 이름, 연락처, 등록일, 만료일 등 핵심 정보만 입력받도록 설계했습니다.  
회원 등록 시에 Strict TypeForm 보다는 String 으로 Data를 받아 등록,수정이 빠르고 편리합니다.

이를 통해 입력 속도와 관리 효율을 높였습니다.

---

### 3. 단일 Member 테이블 구조
회원 정보를 단일 테이블로 관리하여 구조를 단순화하고,  
관리 및 유지보수 비용을 줄이는 방향으로 설계했습니다.

---

## ⚙️ Core Features

### ✔ 회원 등록 / 수정 / 삭제
- 최소 입력값 기반 빠른 회원 등록
- 수정 및 삭제 기능 제공

---

### ✔ 재등록 대상 회원 조회
- 현재 날짜 기준 만료일 ±15일 범위 회원 자동 조회
- 재등록 대상 리스트 제공

---

### ✔ 관리자 편의 기능
- 외부 채널(인스타그램, 네이버 플레이스 등) 연결
- 관리 중심 UI 구성

---

## 🔧 Technical Challenges

### 1. 날짜 기반 동적 데이터 처리
- `remDays`를 DB에 저장하지 않고 계산하도록 설계
- 데이터 정합성과 실시간성을 동시에 확보

---

### 2. 재등록 대상 필터링 로직 구현
- `expireMembersBetweenDates` 로직을 통해  
  특정 기간 내 만료 회원을 효율적으로 조회

---

### 3. React + Spring Boot 통합
- React 빌드 결과물을 Spring Boot static 리소스로 통합
- 프론트엔드와 백엔드의 연결 구조 설계

---

### 4. Docker 기반 실행 환경 구성
- 컨테이너 기반 실행 환경 구성
- 개발 및 배포 환경 일관성 확보

---

## 📡 API Overview

- `GET /members/expireMembers`  
  → 만료일 기준 재등록 대상 회원 조회  

- `POST /members/new`  
  → 최소 입력값 기반 회원 등록  

- `POST /member/modify/{id}`  
  → 회원 정보 수정  

---

## 🛠 Tech Stack

- Backend: Spring Boot, Java  
- Frontend: React, Vite  
- Database: H2  
- Infra: Docker  

---

## 🚀 Future Improvements

- 재등록 임박 회원 알림 기능 (모달 / 팝업)
- To-Do List 기능 추가
- 게시판 기능을 통한 내부 협업 도구 확장

---

## Overview

<h2>Skills</h2>
<img src="https://github.com/hari1010haley/ZeroSugarProject/assets/153698072/7c8ed8fb-f846-47af-8ab7-e42796ee0394" alt="리액트+스프링부트">

  Spring-Boot  Java  JavaScript
  Vite  Node.js  React
  H2  Docker 

<br><br>   
<h2>Installation</h2>
[spring.io] 
  gradle, thyneleaf 의존성 설정
[Vite+React]
  npm install -g create-vite
  create-vite my-react-app --template react
  cd my-react-app
  npm run dev
  npm run build
  cp -r build/* path/to/spring-boot-project/src/main/resources/static/
[H2 connection]
  bin % ./h2.sh
[Docker]
  docker compose -f "docker-compose.yml" up -d --build

<br><br>   
<h2>API Documents</h2>
    path="/" element={<MainPage />}<br>
    @GetMapping("/members")     -> return "members/memberList";<br>
    @GetMapping("/members/new") <br>
    @PostMapping("/members/new") -> return "redirect:/members";<br>
    @GetMapping("/member/info/{id}") -> return "members/memberInfoView"; <br>
    @PostMapping("/member/modify/{id}")  ->  return "redirect:/members"; <br>
    @GetMapping("/members/expireMembers")  -> return "members/expireMembers"; <br>
    @GetMapping("/member/delete")  ->  return "redirect:/members"; <br>

<br><br>   
<h2Trouble Shooting</h2>
 1. remDays type 설정과 데이터 처리여부 결정<br>
 2. expireMembersBetweenDates 로직 처리<br>
 3. React build 후 SpringBoot와 통합연결<br>
 4. 배포

---

## Etc )

<h2>References</h2>
참조사이트: None.
개선을 위한 사이트: <a href="https://studiomate.kr/">https://studiomate.kr/</a> 

<br>
🔗 <h3>배포 링크</h3> : www.healingcare.co.kr  -> 현재 인증서 만료

🗓️ <h3>2024.05-.01 - 2024.05.31</h3>
📑 <h3>PPT</h3>
[MemberManagingSystem.pdf](https://github.com/user-attachments/files/15512561/MemberManagingSystem.pdf)


<br>  
🖥️ <h3>프로젝트 시연영상</h3>
메인화면 
https://github.com/hari1010haley/ZeroSugarProject/assets/153698072/6a985183-b290-4e41-8ec3-595aa123df76
회원등록
https://github.com/hari1010haley/ZeroSugarProject/assets/153698072/c5e80394-b0b0-4d51-8950-d07537a68f07
회원정보 수정/삭제
https://github.com/hari1010haley/ZeroSugarProject/assets/153698072/56a83343-30f6-4108-9ed3-a83c8efced6c
재등록멤버확인 

---

## 👤 Author

김하림  
- GitHub: https://github.com/hari1010haley  
- Email: rlagkfla09@gmail.com
