# 안녕하세요, 새로운 영역으로 빠르게 건너가는 개발자 강지희입니다.

> **"배우는 방법을 아는 사람"**

8살부터 22살까지 15년간 쇼트트랙 선수로 활동했고, 고등학교 2학년 때 국가대표였습니다.

주 종목을 이어가면서도 중학교 때는 인라인, 고등학교 1학년 때는 복싱으로 출전했습니다.
빙상에서 격투기로, 자기 몸을 다루는 종목에서 말을 다루는 승마까지 —
**원리가 전혀 다른 영역의 규칙을 빠르게 파악해 몸에 익히는 일**을 반복해 왔습니다.

2025년 12월, 국비 교육원에서 개발을 시작했습니다.
특별한 계기 없이 "재밌어 보여서" 시작했지만, 만든 만큼 결과가 눈에 보인다는 점이 운동과 닮아 있어 계속하게 되었습니다.

Java에서 TypeScript로, 백엔드에서 프론트엔드로, SQL에서 외부 API 연동과 AI까지 —
지금도 익숙하지 않은 영역으로 건너가는 중이고, 그 과정이 낯설지 않습니다.

<br>

## 🛠 기술 스택

### Backend
![Java](https://img.shields.io/badge/Java-17-007396?style=flat&logo=java)
![Spring Boot](https://img.shields.io/badge/Spring_Boot-6DB33F?style=flat&logo=springboot&logoColor=white)
![MyBatis](https://img.shields.io/badge/MyBatis-000000?style=flat)

### Frontend
![React](https://img.shields.io/badge/React-61DAFB?style=flat&logo=react&logoColor=black)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat&logo=typescript&logoColor=white)
![JSP](https://img.shields.io/badge/JSP-007396?style=flat)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat&logo=javascript&logoColor=black)

### Database
![MySQL](https://img.shields.io/badge/MySQL-8.0-4479A1?style=flat&logo=mysql&logoColor=white)

### Infra / Tools
![AWS](https://img.shields.io/badge/AWS-232F3E?style=flat&logo=amazonaws)
![GitHub](https://img.shields.io/badge/GitHub-181717?style=flat&logo=github)
![Eclipse](https://img.shields.io/badge/Eclipse-2C2255?style=flat&logo=eclipseide)

### AI / API
![Gemini](https://img.shields.io/badge/Google_Gemini-8E75B2?style=flat&logo=googlegemini&logoColor=white)

<br>

## 📂 Projects

### 1. Bridgify — 미국 주식 실질가치 투자 시뮬레이터

**개인 프로젝트 · 풀스택**

2026.05 세미 프로젝트로 시작 → 2026.07 수료 후 개인 프로젝트로 확장 (진행 중)

<img width="1904" height="825" alt="Image" src="https://github.com/user-attachments/assets/e8b0c739-342e-47b6-9c5f-db87022bfa57" />

<img width="1879" height="946" alt="Image" src="https://github.com/user-attachments/assets/8ca969cd-8f14-4a6f-9c77-ba28852b0e67" />

#### 개발 목표

명목 수익률은 높아 보여도, 세금과 물가를 반영하면 실제 구매력은 크게 달라집니다.
미국 주식에 원화로 투자할 때 **실제로 손에 남는 돈**이 얼마인지를 계산하는 시뮬레이터를 만들었습니다.

사용자가 직접 입력해야 하는 값(물가·환율·현재가·배당)을 전부 외부 데이터로 자동 조회해,
**종목과 매수 정보만 넣으면 나머지는 자동으로 계산되는 것**을 목표로 했습니다.

교육 과정의 세미 프로젝트로 시작했으나 기간 내에 완성하지 못했고,
수료 후 배당 재투자·외부 API 캐싱·AI 해설 등을 직접 학습해 추가하며 계속 만들고 있습니다.

#### 기술 스택

| 구분 | 내용 |
|------|------|
| Backend | Spring Boot 4.0.6, MyBatis, Java 17 |
| Frontend | React 19, TypeScript, Zustand, Vite, Recharts |
| Database | MySQL 8.0 |
| External API | Finnhub(주가), 한국수출입은행(환율), Alpha Vantage(배당), FRED(미국 CPI), 한국은행 ECOS(한국 CPI) |
| AI | Google Gemini (gemini-2.5-flash-lite) |

#### 주요 구현 기능

- 복리 기반 미래 자산 시뮬레이션 및 실질가치 환산 (`PV = FV / (1 + r)^n`)
- **DRIP(배당 재투자)** — 매년 받은 배당으로 그 해 주가에 주식을 추가 매수, 복리 효과 반영
- 실현손익 정산 — 과거 매수 정보로 현재 순수익 계산 (양도소득세 250만 원 공제, 배당소득세 15.4%)
- 매수일 기준 **과거 환율 자동 조회**로 실제 매수 원가 산출
- FRED / ECOS API 연동 실시간 물가상승률 자동 반영
- Gemini API 연동 시뮬레이션 결과 AI 해설
- 명목 vs 실질 성장 그래프, 세금 구성 시각화 (Recharts)

#### 기술적 의사결정

**① 무료 API 호출 제한(하루 25회)을 캐싱으로 극복**

배당·과거주가는 한 번 확정되면 바뀌지 않는 데이터라는 점에 착안해,
**온디맨드 조회 + DB 영구 캐싱** 구조를 설계했습니다.

```
배당 데이터 요청
  ├─ DB에 있음  → 즉시 반환 (API 호출 0회)
  └─ DB에 없음  → 외부 API 1회 호출 → 연 단위 가공 → DB 저장
                   (이후 같은 종목은 영구히 DB에서 처리)
```

종목당 평생 1회만 호출하므로, "하루 25회" 제한이 "하루에 새로 등장하는 종목 25개"를 의미하게 됩니다.
한 번의 API 호출로 주가와 배당을 동시에 수집해 호출 수를 절반으로 줄였고,
데이터 소스 교체가 필요하면 한 클래스만 수정하면 되도록 분리했습니다.

**② 물가 이중 차감 방지**

미국 주식을 원화로 투자하면 물가가 두 번 작용하는 것처럼 보이지만, 두 물가를 모두 차감하면 이중 차감이 됩니다.

| 요소 | 반영되는 곳 |
|------|-------------|
| 미국 물가 | 기업 실적 → 주가(수익률)에 이미 포함 |
| 미·한 물가 차이 | 장기적으로 환율에 반영 |
| **한국 물가** | **최종 원화의 구매력 차감 ← 직접 나누는 지점** |

계산은 한국 물가만 사용하되, 사용자가 오해하지 않도록 **반영 경로를 UI에 명시**했습니다.

**③ AI 프롬프트는 백엔드에서 조립**

프론트에서 프롬프트 문자열을 만들어 보내면 사용자가 요청을 조작해 임의의 지시를 주입할 수 있습니다(프롬프트 인젝션).
프론트는 **숫자 데이터만** 전송하고, 프롬프트 문장은 서버가 조립하도록 설계했습니다.
API 키는 환경변수로만 관리해 브라우저에 노출되지 않습니다.

**④ 렌더링 성능 문제 해결**

- 종목 입력 시 커서가 사라지는 문제 → `key`에 입력값(ticker)이 포함되어 매 입력마다 컴포넌트가 remount되던 것이 원인. `key`를 안정적인 값으로 교체
- Zustand 선택 구독(`useStore((s) => s.field)`)과 `React.memo`로 불필요한 차트 리렌더 차단

📁 [Bridgify 소스코드](https://github.com/kangjihee-97/Bridgify)

<br>

---

### 2. VERNALIS ATS — 채용 지원자 관리 시스템

**팀 프로젝트 (3인) · 2026.05.27 ~ 2026.07.03**

<img width="1000" alt="VERNALIS 통계 리포트 - KPI 및 차트" src="https://github.com/user-attachments/assets/be00c33d-6bb3-478a-ac60-aeaa2e1dab42" />

<img width="1000" alt="VERNALIS 통계 리포트 - 퍼널 및 AI 인사이트" src="https://github.com/user-attachments/assets/2a014104-2bbe-4682-9e4d-ab103c6d58ae" />

> 담당 구현: 채용 통계 리포트 (집계 쿼리 설계 · Chart.js 시각화 · Gemini AI 인사이트)

#### 개발 목표

채용 담당자가 지원자를 효율적으로 관리할 수 있는 파이프라인 기반 웹 시스템.
서류 접수부터 최종 합격까지의 단계 전이를 칸반 보드로 시각화하고,
AI 기반 이력서 분석과 채용 통계 리포트를 제공합니다.

#### 기술 스택

| 구분 | 내용 |
|------|------|
| Backend | Spring Boot, MyBatis, Java 17 |
| Frontend | JSP, JavaScript, jQuery, Ajax, Chart.js |
| Database | MySQL 8.0 |
| AI / API | Google Gemini API |
| 보안 | spring-security-crypto (BCrypt) |
| 배포 | AWS EC2 |
| 협업 | GitHub (develop 브랜치 기반) |

#### 담당 영역

**📊 채용 통계 리포트 설계 및 구현**
- 합격률, 평균 채용 소요일, 단계별 지원자 분포, 공고별 합격/불합격 현황 등 집계 쿼리 설계
- **채용 퍼널 이탈률** 분석 — 단계별 전환율을 계산해 채용 병목 구간을 시각화
- 불합격 사유 TOP 5 집계
- Chart.js 기반 도넛 차트 / 누적 막대 차트 / 커스텀 퍼널 렌더링
- **Gemini API 연동 AI 채용 인사이트** — 집계된 통계를 프롬프트로 구성해 자연어 분석 리포트 생성

**🔐 이메일 2단계 인증**
- 인증 코드 생성 · 발송 · 검증 · 만료 처리
- 멘토 피드백 반영: 예측 가능한 값 대신 암호학적 난수(`SecureRandom`)로 코드 생성

**🔄 파이프라인 비동기 상태 전이**
- 칸반 보드 드래그 앤 드롭 UI
- Ajax 비동기 요청으로 페이지 새로고침 없이 단계 전이
- 상태 변경과 `stage_history` 이력 기록을 `@Transactional`로 묶어 **정합성 보장**
  (단계는 바뀌었는데 이력이 누락되는 상황 방지)

📁 [VERNALIS 소스코드](https://github.com/kangjihee-97/ATS_Project)

<br>

---

## 📜 기타

| 활동 | 내용 | 시기 |
|------|------|------|
| 쇼트트랙 국가대표 | 15년간 선수 활동 (8세 ~ 22세), 고교 2학년 국가대표 | — |
| 다종목 출전 | 인라인(중학교), 복싱(고교 1학년) — 단기 준비 후 출전 | — |
| 국비 교육 수료 | Java 백엔드 / 풀스택 과정 | 2025.12 ~ 2026.07 |

<br>

---

운동을 하며 배운 건 **모르는 걸 익히는 순서**였습니다.
낯선 규칙을 파악하고, 반복해서 몸에 붙이고, 다음 영역으로 건너가는 일.

개발도 다르지 않았습니다. 앞으로도 계속 건너가겠습니다.
