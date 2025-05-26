# 💡 프로젝트 소개

![main](https://github.com/user-attachments/assets/c516fbc0-e5f4-4735-af53-586cf56ce7ba)

## ⚙ 개요
- **진행 기간**: 2025/2/27 ~ 2025/4/11
- **소개**: 음성 명령 기반 폰뱅킹 서비스

## ⚙ 기획 배경
- 최근 국내 은행들의 비대면 업무가 증가하면서 노년층의 디지털 소외 문제가 부각됨
- 복잡한 은행 앱의 UI를 개선하고, 음성 인식을 통해 금융 업무를 쉽게 수행할 수 있도록 지원

## ⚙ 팀 구성
| 이름     | 포지션 | 역할 |
|----------|-------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 박가희   | FE, 팀장 | 계좌 생성 화면 개발, 음성 시나리오 작성, 앱 초기 화면 분기 로직, 회원가입·로그인·계좌 이체 기능 API 연동 및 뷰모델 연계 |
| 김고은   | FE    | 신분증 OCR, 공과금 납부, 회원가입 및 계좌이체 화면 디자인 |
| 최은진   | FE    | 음성 라우팅, 계좌 목록 조회, 푸시알림 |
| 김성찬   | AI    | 의도 추론, 화자 식별, 스푸핑 탐지 |
| 김시아   | BE    | 계좌 생성/이체/해지, 계좌 정보 조회/수정, FCM, 이상거래 탐지 |
| 이영석   | BE    | Infra, CI/CD, 회원 관련 인증 및 유저 서비스, S3 연동 |

## ⚙ 개발 환경 및 기술
![tech](https://github.com/user-attachments/assets/094e1d1b-9396-4322-a97f-4d92371e16e0)

## ⚙ API 명세서
- [Notion 문서](https://www.notion.so/Rest-API-1ac1f58b9939808c87dbd4798d9ec76d)
- [Postman 문서](https://documenter.getpostman.com/view/41511989/2sB2cYbKnX)

## ⚙ ERD
<img width="600" src="https://github.com/user-attachments/assets/3a9c905e-60c8-4b83-8b0b-158e421572e3">

## ⚙ 아키텍처
<img width="1000" src="https://github.com/user-attachments/assets/b4778c33-ca53-46af-8558-b0353c94c7da">

## 🆚 서비스 소개

### 🔐 회원가입 및 로그인
- **회원가입**: 핸드폰 인증, OCR 신분증 인증
- **로그인**: JWT, Spring Security 기반

### 💳 계좌 생성
<img width="500" src="https://github.com/user-attachments/assets/acd33c4b-6a0e-435b-a852-cb2c0bf542b8">

- "계좌 생성해줘" 음성 명령으로 페이지 이동

### 📄 계좌 목록 조회
<img width="500" src="https://github.com/user-attachments/assets/f1ed5b15-478a-42f9-a072-2825436f06c8">

- "계좌 목록 조회해줘" 음성 명령으로 페이지 이동

### 💸 계좌 이체 (본인)
<img width="500" src="https://github.com/user-attachments/assets/907eabf6-4898-4d39-bddb-475f0fb985bd">

- 화자 인증으로 본인 여부를 검증, 본인일 경우 이체 성공

### ❌ 계좌 이체 (타인)
<img width="500" src="https://github.com/user-attachments/assets/648406d0-0e76-496e-ba8b-d48487aa241d">

- 타인으로 판단되면 음성 인증 실패 → 비밀번호 인증 단계로 전환

### 💡 공과금 납부
<img width="500" src="README_assets/bill.gif">

- OCR로 공과금 납부

### 📲 FCM 알림
<img width="500" src="https://github.com/user-attachments/assets/576b4f78-049e-49b8-8dc6-2b557b76b8ed">

- 계좌 입금 시 푸시 알람 전송

---

## 🧠 Machine Learning

### ⚙ 이상거래 탐지
- **Isolation Forest**: 비지도 이상거래 탐지
- **재학습**: MongoDB 거래 내역을 매일 자정 Spring Scheduler로 재학습 (FastAPI 호출)
- 신규 유저 cold start 방지를 위해 약 1만 건 거래 내역 데이터로 사전 학습

### ⚙ AI
- **의도 추론**: KF-DeBERTa, 금융 의도 8개로 분류, LLM+정규식으로 엔티티 추출
- **화자 식별**: ECAPA-TDNN (VoxCeleb2), cosine 유사도 기반 검증
- **스푸핑 탐지**: RawNet2, ASVspoof 2019 데이터셋 기반으로 위조 여부 탐지

---

## 🏗️ MSA 구조

### ⚙ Saga Pattern
- 마이크로서비스간 트랜잭션 조정
- 회원가입 등에서 Auth 도메인 오류 발생 시 User 도메인 데이터 롤백

<img width="800" src="https://github.com/user-attachments/assets/698d77ef-647d-4d34-a685-dc74d075ba3f">
<img width="800" src="https://github.com/user-attachments/assets/fd7d03dd-2b96-47c8-8edc-63525023a51b">
<img width="800" src="https://github.com/user-attachments/assets/2b5e4893-f91a-4779-bacd-7e56270ffbc2">

### ⚙ Circuit breaker Pattern
- Resilience4j로 장애 격리 및 에러 전파 차단

<img width="800" src="https://github.com/user-attachments/assets/0971f8c5-f78e-423b-a4b0-ea90346535a7">

### ⚙ 부하 테스트
- K6, InfluxDB, Grafana로 10분간 500명, 약 9만건 요청
- TPS: 140, 평균 응답: 1.26초

<img width="800" src="https://github.com/user-attachments/assets/3b00b429-c810-4bdd-9c13-b89e1f9000eb">

---

## ⚠️ PR Template

<details>
<summary>열기/닫기</summary>

```declarative
# [FE] merge: from feature/ to dev

## 🤷‍♂️ Description
<!-- 기능을 설명해주세요. -->

## 🔎 PR 유형
어떤 변경 사항이 있나요?

- [ ] 기능 및 코드 추가
- [ ] 기능 및 코드 수정 완료
- [ ] 버그 수정
- [ ] 디자인 수정
- [ ] 코드 리팩토링
- [ ] 문서작성 및 편집
- [ ] config 파일 수정
- [ ] 파일 추가
- [ ] 파일 제거
- [ ] 오타 수정
- [ ] 프로젝트 구조 변경

## 📸 Screenshots
<!-- 필요한 경우 사진을 남겨주세요. -->

## ✳️ Remarks
<!-- 비고란으로 Optional 입니다. 필요시 수정해주세요. -->

## ➕ 지라 링크
- [지라번호-숫자](지라주소)

## 🧐 PR Checklist
- [ ] 커밋 메시지 & 브랜치 컨벤션에 맞게 작성했습니다.
- [ ] 변경 사항에 대한 테스트를 했습니다.
- [ ] 변경 사항에 대한 빌드에 성공했습니다.


## ⚠️ Commit Convention


| 커밋 유형 | 의미 |
| :---: | :---: |
| `feat` | 새로운 기능에 대한 커밋 |
| `fix` | 버그 수정에 대한 커밋 |
| `build` | 빌드 관련 파일 수정 / 모듈 설치 또는 삭제에 대한 커밋 |
| `chore` | 그 외 자잘한 수정에 대한 커밋 |
| `ci` | ci 관련 설정 수정에 대한 커밋 |
| `docs` | 문서 수정에 대한 커밋 |
| `style` | 코드 스타일 혹은 포맷, 단순 디자인 변경 등 |
| `refactor` | 코드 리팩토링에 대한 커밋 |
| `test` | 테스트 코드 수정에 대한 커밋 |
| `perf` | 성능 개선에 대한 커밋 |
| `add` | 파일 추가에 대한 커밋 |
| `remove` | 파일 삭제에 대한 커밋 |
