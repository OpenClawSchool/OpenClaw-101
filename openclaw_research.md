# OpenClaw 리서치 노트

## 메타 정보
- **조사일:** 2026-03-16
- **토픽:** OpenClaw — 오픈소스 AI 에이전트 플랫폼
- **목적:** OpenClaw School 교육 자료 및 배경 지식 확보
- **범위:** 개념, 아키텍처, 생태계, 보안, 경쟁 비교, 한국 커뮤니티 현황
- **출처 수:** 30+ 소스

---

## Concept (개념)

### 📖 OpenClaw란?

OpenClaw는 **AI 에이전트를 만들고 실행할 수 있는 오픈소스 플랫폼**이다. 단순 챗봇이 아니라, 사용자의 명령을 이해하고 **컴퓨터에서 실제 작업을 자율적으로 수행**하는 개인 AI 비서 런타임이다.

> **공식 슬로건:** "The AI that actually does things."

| 항목 | 내용 |
|------|------|
| **정식 명칭** | OpenClaw |
| **유형** | 오픈소스 AI 에이전트 플랫폼 / 런타임 |
| **라이선스** | MIT License |
| **개발자** | Peter Steinberger (오스트리아) |
| **최초 출시** | 2025년 11월 (Clawdbot → Moltbot → OpenClaw) |
| **현재 버전** | v1.0 beta (2026년 기준) |
| **런타임** | Node.js |
| **공식 사이트** | [openclaw.ai](https://openclaw.ai) |
| **GitHub** | [github.com/openclaw/openclaw](https://github.com/openclaw/openclaw) |

### 이름 변천사

```
Clawdbot (2025.11) → Moltbot → OpenClaw (2026.01)
(상표 이슈 + 브랜딩 → 최종 OpenClaw로 확정)
```

### 창립자 근황

- 2026년 2월, 창립자 Peter Steinberger가 **OpenAI에 합류** (Personal Agents 팀)
- OpenClaw 프로젝트는 **독립 오픈소스 재단**으로 이관 → 커뮤니티 주도 운영

---

## Key Findings (핵심 발견)

### 📊 주요 데이터 & 수치

| 지표 | 수치 | 출처 |
|------|------|------|
| GitHub Stars | 대규모 (커뮤니티 급성장) | GitHub |
| ClawHub 등록 스킬 | 수천 개 (2026년 기준) | TencentCloud |
| 코드베이스 규모 | 400,000줄+ | ZDNet |
| 지원 메신저 | 6개+ (WhatsApp, Telegram, Discord, Slack, Signal, iMessage) | openclaw.ai |
| 지원 LLM | Claude, GPT, Gemini, DeepSeek, Ollama 등 다수 | 공식 문서 |
| 악성 스킬 발견 건수 | 230개+ (ClawHub) | Kaspersky |
| 노출된 인스턴스 | 수만 개 (미인증 상태) | TrendMicro |
| Moltbook 유출 토큰 | 150만 개 (API 토큰 + 비공개 대화) | TrendMicro |
| 보안 취약점 CVE | CVE-2026-25253 (CVSS 8.8) | DigitalOcean |

---

### 🏗️ 아키텍처 & 핵심 기능

#### 3대 핵심 구성 요소

```
┌─────────────────────────────────────────────┐
│                  OpenClaw                   │
│                                             │
│  ┌───────────┐  ┌────────────────────────┐  │
│  │  Gateway   │  │   Browser Relay        │  │
│  │ (명령 수신) │  │ (브라우저 자동화 실행)  │  │
│  └─────┬─────┘  └───────────┬────────────┘  │
│        │                    │               │
│        └────────┬───────────┘               │
│                 ▼                           │
│        ┌──────────────┐                     │
│        │  Agent Core  │                     │
│        │ (에이전트 로직)│                     │
│        └──────────────┘                     │
└─────────────────────────────────────────────┘
```

| 구성 요소 | 역할 | 쉬운 비유 |
|-----------|------|-----------|
| **Gateway** | 외부 메시지(Telegram 등)를 수신하여 에이전트에게 전달 | 회사의 안내 데스크 |
| **Browser Relay** | 에이전트가 웹 브라우저를 조작할 수 있게 해주는 장치 | 에이전트의 손 |
| **Agent Core** | 실제로 생각하고 판단하는 에이전트의 두뇌 | 직원의 머리 |

#### 6대 핵심 기능

| 기능 | 설명 |
|------|------|
| **🖥️ 로컬 실행** | Mac, Windows, Linux에서 자체 호스팅. 데이터가 사용자 기기에 보관 |
| **💬 멀티 메신저** | WhatsApp, Telegram, Discord, Slack, Signal, iMessage 지원. DM + 그룹 채팅 |
| **🧠 영구 기억** | 사용자 선호, 맥락을 Markdown 파일로 로컬 저장. 모델 변경 후에도 대화 유지 |
| **🌐 브라우저 제어** | 웹 브라우징, 폼 작성, 데이터 추출 가능 |
| **⚙️ 시스템 접근** | 파일 읽기/쓰기, 셸 명령, 스크립트 실행. 풀 접근 또는 샌드박스 선택 |
| **🔌 스킬 & 플러그인** | 커뮤니티 스킬 설치 또는 직접 제작. 에이전트가 스스로 스킬 작성 가능 |

#### 동작 흐름

```
사용자가 Telegram에서 메시지 전송
  → Gateway가 메시지를 수신
    → Agent Core가 메시지를 분석하고 판단
      → 필요 시 Browser Relay로 웹 작업 수행
        → 결과를 사용자에게 응답
```

---

### ⚖️ 비교 분석

#### OpenClaw vs LangChain (2025~2026)

| 비교 항목 | OpenClaw | LangChain |
|-----------|----------|-----------|
| **핵심 성격** | AI 에이전트 **런타임 환경** (OS급) | AI 앱 개발 **라이브러리** (코드 도구) |
| **목적** | 실제 시스템에서 자율 행동 수행 | AI 기반 소프트웨어 구축 |
| **대상 사용자** | 비개발자~일반 사용자 | 소프트웨어 엔지니어 |
| **설치 방식** | 서버 1대에 간단 설치 (원라이너) | pip/npm 라이브러리 설치 |
| **메신저 연동** | 6개+ 기본 내장 | 직접 구현 필요 |
| **브라우저 자동화** | 내장 (Browser Relay) | 별도 구현 |
| **메모리** | 영구 기억 내장 (로컬 Markdown) | Vector Store 직접 구성 |
| **모델 호환** | 모델 불문 (Claude, GPT, Gemini, 로컬 등) | 1000+ 통합 |
| **확장 방식** | 스킬/플러그인 마켓플레이스 (ClawHub) | 체인/에이전트 코드 |
| **성숙도** | v1.0 beta (실험적) | 안정 릴리스 (프로덕션급) |
| **코드베이스** | 400,000줄+ | 모듈형 라이브러리 |
| **난이도** | ⭐⭐ (초보자 친화) | ⭐⭐⭐⭐ (개발자 대상) |
| **라이선스** | MIT (무료) | MIT (무료) |

> **핵심 차이:** LangChain은 "AI로 소프트웨어를 **만드는** 도구", OpenClaw는 "AI가 실제로 **일하는** 환경"

#### OpenClaw vs NanoClaw (보안 관점)

| 비교 항목 | OpenClaw | NanoClaw |
|-----------|----------|----------|
| **철학** | 기능 우선 (풀 시스템 접근) | 보안 우선 (격리 기반) |
| **코드 규모** | 400,000줄+ | 4,000줄 미만 |
| **격리 수준** | 제한적 (에이전트 간 격리 부족) | 컨테이너 + MicroVM 2중 격리 |
| **스킬 권한** | 항상 활성 / 과도한 권한 | 필요 시에만 접근 |
| **기반** | 자체 아키텍처 | Anthropic Claude Code 기반 |
| **감사 용이성** | 어려움 (대규모 코드) | 용이 (소규모 코드) |

---

### 🔌 생태계: ClawHub & 스킬 시스템

#### ClawHub (스킬 마켓플레이스)

- 커뮤니티가 만든 **수천 개의 스킬**을 공유하는 마켓플레이스
- 스킬 = `SKILL.md` (마크다운 기반 지시서) + 메타데이터로 구성된 패키지
- 에이전트가 새로운 도구, API, 워크플로우를 사용할 수 있게 확장

#### 인기 스킬 (2026년 기준)

| 순위 | 스킬 카테고리 | 예시 |
|------|-------------|------|
| 1 | 웹 브라우징 | 웹 검색, 데이터 스크래핑 |
| 2 | Telegram 통합 | 봇 자동 응답, 그룹 관리 |
| 3 | 이메일 관리 | Gmail 읽기/발송/요약 |
| 4 | 데이터베이스 쿼리 | SQL 실행, 데이터 분석 |
| 5 | 비즈니스 자동화 | CRM 연동, 보고서 생성 |

#### 보안 대응

- 2026년 2월, **VirusTotal과 파트너십** → ClawHub에 등록되는 모든 스킬 보안 스캔
- Tencent Cloud가 OpenClaw 커뮤니티 **스폰서**로 참여 (2026년 3월)

---

### 🛠️ 활용 사례

| 분야 | 활용 방법 | 구체적 예시 |
|------|----------|------------|
| **업무 자동화** | 반복 작업 자동 처리 | 수천 개 영수증 이미지 분석, 코드 리뷰 요약 |
| **개인 비서** | 24시간 상시 실행 비서 | 이메일 확인, 웹사이트 모니터링, 메신저 알림 |
| **DevOps** | 개발 파이프라인 자동화 | Telegram→코드 빌드→GitHub 푸시, 에러 로그 감시 |
| **원격 제어** | 스마트폰으로 PC 원격 조작 | 퇴근길에 회의록 정리 및 메일 발송 지시 |
| **스케줄링** | 시간 기반 자동 실행 | 문서 동기화, CRM 스캔, 아침 브리핑 준비 |
| **팀 협업** | Slack/Discord 연동 비서 | 반복 질문 응대, 프로젝트 현황 공유 |

---

### 🔮 트렌드 & 전망

| 트렌드 | 상세 |
|--------|------|
| **오픈소스 재단 전환** | 창립자 OpenAI 합류 → 독립 재단으로 이관, 커뮤니티 주도 개발 가속 |
| **보안 강화** | VirusTotal 파트너십, 보안 감사 도구(`openclaw doctor`) 도입 |
| **클라우드 배포** | Amazon Lightsail + Bedrock 프리컨피그, Tencent Cloud 스폰서 |
| **로컬 AI 실행** | AMD Ryzen AI Max+ 등에서 로컬 LLM 실행 최적화 |
| **경쟁 심화** | NanoClaw(보안 중심), LangChain/LangGraph(개발자 중심)와 포지셔닝 분화 |
| **아시아 확산** | 한국(OpenClaw Korea), 중국("랍스터 키우기" 열풍) 커뮤니티 활성화 |
| **카카오톡 플러그인** | 한국 커뮤니티에서 카카오톡 연동 플러그인 개발 중 |

---

### ⚠️ 한계 & 보안 고려사항

#### 주요 보안 위험

| 위험 | 상세 | 심각도 |
|------|------|--------|
| **RCE 취약점** | CVE-2026-25253 — WebSocket 하이재킹을 통한 원격 코드 실행 | 🔴 CVSS 8.8 |
| **악성 스킬** | ClawHub에 악성 스크립트 230개+ 유포 | 🔴 높음 |
| **프롬프트 인젝션** | 간접 프롬프트 주입으로 비정상 행동 유발 가능 | 🟠 중간 |
| **인스턴스 노출** | 수만 개 인스턴스가 인증 없이 공개 노출 | 🔴 높음 |
| **평문 저장** | API 키, WhatsApp 메시지 등이 평문으로 저장 | 🟠 중간 |
| **에이전트 간 격리 부족** | 다른 에이전트/사용자 데이터에 접근 가능 | 🟠 중간 |
| **Moltbook 유출** | 150만 API 토큰 + 비공개 대화 유출 | 🔴 높음 |

#### 기업 대응 현황

| 기업/기관 | 조치 |
|-----------|------|
| 네이버, 카카오, 당근마켓 | 사내망/업무기기 사용 **금지·제한** |
| 중국 인터넷금융협회 | 금융기관 대상 공식 **사용 경고** |
| 중국 정부 | 사이버 공격 통로로 간주, **사용 금지** |

#### 안전한 사용 권장 사항

```
✅ 격리된 환경(VM 또는 Docker)에서 실행
✅ openclaw doctor 보안 감사 명령 실행
✅ 서드파티 스킬 설치 전 코드 직접 검토
✅ 중요 데이터가 없는 별도 컴퓨터에서 사용
✅ 비특권 계정(non-root)으로 실행
✅ 외부 접근(포트 노출) 최소화
```

---

### 🇰🇷 한국 커뮤니티 현황

| 항목 | 내용 |
|------|------|
| **OpenClaw Korea** | GitHub 오픈소스 커뮤니티 (7개 저장소 운영) |
| **GPTers** | Slack 연동 비서 활용 사례 공유 |
| **IT 미디어** | 한국경제, ZDNet, 매경, 동아 등 다수 보도 |
| **블로그** | Tistory, Brunch, Wishket 등에서 설치/활용 가이드 공유 |
| **현지화 프로젝트** | 카카오톡 플러그인 개발 중 (Reddit 커뮤니티) |

---

## Example (사례)

### 빠른 설치 (Quick Start)

```bash
# macOS/Linux 원라이너 설치
curl -fsSL https://openclaw.ai/install.sh | bash

# 온보딩 마법사 실행
openclaw onboard --install-daemon

# AI 모델(두뇌) 연결
openclaw configure --section model

# 메신저 채널 연결
openclaw configure --section channels

# 웹 검색 기능 연결
openclaw configure --section web

# Gateway 재시작 (설정 반영)
openclaw gateway restart
```

### 에이전트에게 할 수 있는 명령 예시

```
"내일 일정 알려줘"
"이번 주 비어있는 시간 찾아줘"
"3월 15일 오후 2시에 미팅 일정 추가해줘"
"오늘 온 중요 메일 요약해줘"
"인스타그램 광고 카피 5개 만들어줘"
```

### 시스템 요구 사항

| 항목 | 최소 | 권장 |
|------|------|------|
| **OS** | macOS / Linux / Windows (WSL2) | macOS / Linux |
| **Node.js** | v18+ | v22 LTS 또는 v24 |
| **RAM** | 2GB | 4GB+ |
| **저장 공간** | 1GB | 5GB+ |
| **AI API 키** | 1개 (Claude 또는 GPT 등) | OpenRouter로 다수 모델 연동 |

---

## Sources (출처)

### 공식 소스
- [openclaw.ai](https://openclaw.ai) — 공식 홈페이지
- [GitHub - openclaw/openclaw](https://github.com/openclaw/openclaw) — 소스 코드
- [docs.openclaw.ai](https://docs.openclaw.ai) — 공식 문서
- [steipete.me](https://steipete.me) — 창립자 블로그

### 기술 분석
- [DigitalOcean](https://digitalocean.com) — 설치 가이드 및 보안 분석
- [Medium](https://medium.com) — 아키텍처 분석, 커뮤니티 활용 가이드
- [HackerNoon](https://hackernoon.com) — 시스템 요구사항 및 설정 가이드
- [DataCamp](https://datacamp.com) — 종합 가이드

### 비교 분석
- [Reddit r/openclaw](https://reddit.com/r/openclaw) — OpenClaw vs LangChain 토론
- [GrandLinux](https://grandlinux.com) — 프레임워크 비교 분석
- [SparkCo.ai](https://sparkco.ai) — 2026 AI Agent 프레임워크 순위

### 보안
- [Forbes](https://forbes.com) — NanoClaw 보안 비교 분석
- [Microsoft Security](https://microsoft.com) — OpenClaw 보안 가이드라인
- [Kaspersky](https://kaspersky.com) — 악성 스킬 보고서
- [TrendMicro](https://trendmicro.com) — 노출 인스턴스/유출 분석
- [Socket.dev](https://socket.dev) — VirusTotal 파트너십
- [ZDNet](https://zdnet.com) — NanoClaw vs OpenClaw 코드 비교

### 한국 소스
- [한국경제](https://hankyung.com) — OpenClaw 국내 보도
- [ZDNet Korea](https://zdnet.co.kr) — 중국 사용 금지 보도
- [매일경제](https://mk.co.kr) — 보안 이슈 보도
- [GPTers](https://gpters.org) — Slack 비서 활용 사례
- [OpenClaw Korea (GitHub)](https://github.com/orgs/openclaw-korea) — 한국 커뮤니티

---

## Checklist (검증)

- [x] 핵심 개념이 명확히 정의되었는가?
- [x] 데이터/수치의 출처가 명시되어 있는가?
- [x] 다양한 시각(긍정/비판)이 포함되었는가?
- [x] 아키텍처와 구조가 시각적으로 표현되었는가?
- [x] 경쟁 제품(LangChain, NanoClaw)과의 비교가 포함되었는가?
- [x] 보안 위험과 대응 방안이 포함되었는가?
- [x] 한국 커뮤니티 현황이 포함되었는가?
- [x] 에이전트 지식 문서로 활용 가능한 구조인가?
- [x] 최소 5개 이상의 소스에서 자료를 수집했는가?
- [x] OpenClaw 표준 포맷(Concept-Example-Checklist)이 적용되었는가?
