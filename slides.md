# Claude 교육

---

## Slide 1: Claude란? (약 2분)

**할 수 있는 것들**

- 코드 작성 / 분석 / 리팩토링
- 문서 작성 / 요약 / 번역
- 데이터 분석 / 정리
- 테스트 케이스 생성
- 마크업 생성 / 수정
- 기획서 검토 / 피드백

> 코딩만이 아니라, 모든 업무에 활용 가능

---

## Slide 2: Claude 제공 형태 (약 1분)

| | 웹 Claude | Claude Desktop | **Claude Code** |
|---|---|---|---|
| 어디서 | 브라우저 | 데스크톱 앱 | **터미널** |
| 뭘 하냐 | 대화 / 질문 | 대화 + 파일 연동 | **프로젝트 전체에서 직접 작업** |
| 핵심 | 범용 | 편의성 | **자동화** |

> 오늘 볼 건 **Claude Code** — 여러분 프로젝트에서 바로 동작하는 AI 개발 도구입니다.

---

## Slide 3: Claude Code 실행 방법 (약 30초)

```
~/my-project › claude

› JIRA-1234 버그 분석하고 수정해줘▊
```

---

## Slide 4: 알아두면 좋은 기본 개념 (약 2분 30초)

**토큰 (Token)**
- AI가 읽고 쓰는 단위
- 영어는 짧은 단어 하나가 대략 1토큰
- 한글은 같은 의미라도 더 많은 토큰 소모

**컨텍스트 (Context)**
- AI가 답변 생성 시 참고할 수 있는 텍스트의 양
- 대화 내용 + 파일 내용 + 설정 = 전부 컨텍스트에 들어감
- 컨텍스트가 가득 차면 → **대화 내용을 잊어버리거나, 중요한 맥락을 못 짚는 경우 발생**

> 불필요한 정보가 컨텍스트를 차지할수록, AI 성능이 떨어진다.

---

## Slide 5: 컨텍스트가 길어지면 무슨 일이 생기는가? (약 2분)

```
[나쁜 예]
┌─────────────────────────────────────────────────┐
│ 배포 규칙 │ DB 설정 │ ★핵심★ │ API 규칙 │ 로그 설정 │
└─────────────────────────────────────────────────┘
→ 핵심이 불필요한 정보에 묻힘 → AI가 못 찾음

[좋은 예]
┌───────────────┐
│ ★핵심★        │
└───────────────┘
→ 핵심만 있음 → AI가 바로 찾음
```

**Lost in the Middle** — AI는 컨텍스트의 처음과 끝은 잘 잡지만, 중간에 있는 정보는 놓친다 (Stanford, 2023)

> 많이 넣는다고 좋은 게 아니라, 핵심만 넣어야 AI가 제대로 동작한다.

---

## Slide 6: 컨텍스트 관리 — /clear & /compact (약 1분)

**작업이 끝나면 컨텍스트를 비워라**

| 명령어 | 설명 |
|--------|------|
| `/clear` | 대화 초기화 — 새 작업 시작 전에 |
| `/compact` | 대화 요약 압축 — 같은 작업 계속할 때 |

> 작업이 끝나면, 컨텍스트를 비워라.

---

## Slide 7: Claude Code의 핵심 기능들 (약 2분)

| 기능 | 한마디 정의 |
|---|---|
| **CLAUDE.md** | Claude가 자동으로 읽는 기본 지침서 |
| **Skills** | 자주 쓰는 작업을 명령어로 만들어두는 것 (`/deploy`, `/review`) |
| **MCP** | 외부 서비스(Jira, Confluence, DB 등)를 Claude에 연결하는 통로 |
| **Subagents** | 큰 작업을 맡길 수 있는 부하 직원 (결과만 보고받음) |
| **Agent Teams** | 여러 Claude가 팀을 이뤄 동시에 일하는 것 |
| **Hooks** | 특정 상황에 자동으로 실행되는 스크립트 |
| **Plugins** | 위 기능들을 묶어서 팀끼리 공유하는 패키지 |

**폴더 구조:**
```
업무자동화/
├── .claude/
│   ├── settings.json      ← 클로드 설정
│   ├── hooks/             ← Hook 실행 시 사용될 스크립트(코드)
│   │   └── auto-format.sh
│   ├── skills/            ← Skills
│   │   └── review/
│   │       └── SKILL.md
│   ├── agents/            ← Subagents
│   │   └── security-auditor.md
│   └── rules/             ← 추가 규칙
│       └── testing.md
├── .mcp.json              ← MCP 서버 설정
└── CLAUDE.md              ← 기본 지침서
```

---

## Slide 8: CLAUDE.md (약 1분)

프로젝트 루트에 위치하는 **Claude를 위한 설명서**

- 범용적인 규칙이나 전체 구조를 전달
- Claude 실행 시 항상 로드됨
- **200줄 이내 작성 권고**

```
# CLAUDE.md
- 한국어로 응답
- 파일명은 kebab-case
- 커밋 메시지는 conventional commits 형식
```

---

## Slide 9: Plan 모드 (약 2분)

**두 가지 결말** — `Shift + Tab`으로 전환

[Without Plan]
요청 → 바로 코딩 → 의도와 다름 → 원복 → 반복... (시간·토큰 낭비)

[With Plan]
요청 → 계획 수립 → 방향 확인 → 실행 → 한번에 성공

> 코드를 쓰기 전에 방향을 맞추면, 재작업이 사라진다.

---

## Slide 10: 반복되는 작업 (약 1분)

```
월요일:
› 주간 보고서 작성해줘.
  금주 처리 티켓 확인하고, 마크다운 표로 정리해.
  형식은 | 티켓 | 상태 | 요약 | ...

수요일:
› 커밋 메시지 작성해줘.
  conventional commits 형식으로,
  변경사항 분석해서 타입 선택하고...

금요일:
› 코드 리뷰해줘.
  보안, 성능, 에러 처리 체크하고
  개선 사항 표로 정리해...
```

---

## Slide 11: Skills (약 1분)

반복되는 작업을 `/명령어`로 등록

**구조:**
```
.claude/
└── skills/
    └── weekly-report/
        ├── SKILL.md        ← 핵심 지침 (필수)
        ├── reference.md    ← 보조 문서 (선택, 필요 시 로드)
        └── scripts/        ← 필요한 스크립트 추가 (선택)
```

**작성 권고:** SKILL.md는 **500줄 이내**로 간결하게. 상세 내용은 보조 문서로 분리 → 필요할 때만 로드

**SKILL.md 파일 예시:**
```markdown
---
name: weekly-report
description: 주간 보고서를 작성합니다
---

1. 금주 처리 티켓 확인
2. 주간 보고서 초안 생성
```

---

## Slide 12: Skills — 생성 방법과 호출 방법 (약 1분)

**생성 방법 2가지:**

| 방법 | 설명 | 권장 |
|------|------|------|
| **직접 생성** | `.claude/skills/{이름}/SKILL.md` 파일을 직접 작성 | |
| **Skill Creator** | 자동 설치된 공식 플러그인. "스킬 생성해줘"라고 말하면 실행 | **권장** |

**호출 방법:**
- 직접 호출: `/weekly-report`
- 자동 호출: "이번 주 보고서 만들어줘" → Claude가 description 보고 자동 매칭

**예시:**
- `/code-review` → git diff 기반 코드 리뷰
- `/commit` → 커밋 메시지 자동 생성
- `/tc-generator` → 테스트 케이스 도출

---

## Slide 13: 외부 프로그램도 AI로 다루고 싶다면? (약 30초)

[Claude] ← 🤔 → [Jira, Wiki, DB]

💭 지라에서 티켓 목록 조회하고 싶은데...
💭 Wiki에서 결제 서비스 문서 확인하고 싶은데...
💭 DB에서 데이터 조회하고 싶은데...

---

## Slide 14: MCP — Claude에 외부 프로그램을 연결해주는 도구 (약 1분)

**연결 구조:**
Claude Code (Client) → MCP Server (jira-server) → Jira (외부 프로그램)

**등록:** `claude mcp add --scope <scope> <서버명> ...`

| scope | 범위 | 저장 위치 |
|-------|------|----------|
| **local** (기본) | 이 프로젝트, 나만 | `.claude/settings.local.json` |
| **project** | 이 프로젝트, 팀 공유 | `.mcp.json` |
| **user** | 모든 프로젝트, 나만 | `~/.claude.json` |

**사용 가능한 MCP 목록 + 연동 방법:** https://smithery.ai/servers

---

## Slide 15: MCP — 주의사항 (약 1분)

**MCP 목록 확인:** `/mcp`

**주의:** 활성화된 모든 MCP가 제공하는 기능 목록과 사용법이 컨텍스트에 로드됨
→ 예: Notion 15,076 토큰 + Slack 7,961 토큰 = MCP만으로 27,000+ 토큰
→ **미사용 MCP는 항상 꺼둘 것**
→ 필요한 기능만 담은 **커스텀 MCP를 만들어 사용** 권장

---

## Slide 16: 결론만 말해 (약 30초)

```
› Wiki에서 결제 서비스 관련 문서 찾아줘

문서 1 탐색 중... → 컨텍스트에 쌓임
문서 2 탐색 중... → 컨텍스트에 쌓임
문서 3 탐색 중... → 컨텍스트에 쌓임

› 테스트 돌려줘

테스트 로그 출력... → 컨텍스트에 쌓임
...
```

→ 탐색·실행 과정 전부가 내 컨텍스트를 차지한다

---

## Slide 17: Subagents — 구조와 파일 예시 (약 1분)

Subagent에게 위임하면, 별도 컨텍스트에서 탐색하고 **결과 요약만 반환**

**구조:**
```
.claude/
└── agents/
    └── search-agent.md
```

**파일 예시:**
```markdown
---
name: search-agent
description: Confluence에서 문서를 검색합니다
tools: Read, Grep, Glob
model: haiku
---
Confluence에서 문서를 검색하고
핵심 내용만 요약해서 보고합니다.
```

---

## Slide 18: Subagents — 생성 방법과 호출 방법 (약 1분 30초)

**생성 방법:** `/agents` → "Create new agent" 선택

**모델 선택:** 작업 성격에 따라 모델을 다르게 지정하면 비용 절감

| 추천 모델 | 특징 |
|----------|------|
| **Haiku** | 빠르고 저렴 |
| **Sonnet** | 판단력과 비용 균형 |
| **Opus** | 높은 추론 필요 시만 |

**호출 방법:**
- @멘션: `@search-agent 결제 서비스 정책 찾아줘`
- 자연어: `search-agent 써서 찾아줘`
- 자동: Claude가 description 보고 알아서 위임

---

## Slide 19: Subagents — 병렬 작업 (약 1분)

**독립적인 작업은 동시에:**

```
› 이 프로젝트의 보안, 성능, 테스트 커버리지를 분석해줘

● Agent(security-auditor)   ← Haiku
  └ 의존성 취약점, 인증 로직 점검

● Agent(perf-analyzer)      ← Sonnet
  └ N+1 쿼리, 느린 API 탐색

● Agent(test-coverage)      ← Haiku
  └ 미커버 코드 영역 리포트

⏱ 각 2분 × 3개 = 순차 6분 → 병렬 2분
```

---

## Slide 20: Agent Teams — 프롬프트 (약 1분)

**Agent Teams** — 여러 Claude가 동시에 협업

```
에이전트 팀을 구성해서 JIRA-1234를 처리해줘.

- 분석 담당: Jira 티켓 확인하고 원인 파악
- 프론트 담당: 화면 쪽 원인 탐색 및 수정
- 백엔드 담당: API 쪽 원인 탐색 및 수정
- 테스트 담당: 수정사항 기반 테스트 작성

서로 발견한 내용을 공유하면서 진행해.
```

> 혼자 다 안 해도 된다. 팀을 꾸려서 시키면 된다.

---

## Slide 21: Agent Teams — 동작 구조 (약 1분)

**Team Lead** (메인 세션) — Task List로 작업 분배

**Teammates** — 각각 독립 Claude 세션으로 실행, 서로 메시지 교환

**Subagents:** 각자 하고 결과만 반환 | **Agent Teams:** 서로 소통하면서 진행

---

## Slide 22: Hooks — 이벤트에 반응하는 자동 실행 (약 1분 30초)

**Hooks** — 특정 이벤트가 발생할 때 실행할 동작을 정의

**주요 이벤트:**

| 이벤트 | 시점 |
|--------|------|
| **PreToolUse** | 도구 실행 전 |
| **PostToolUse** | 도구 실행 후 |
| **Stop** | Claude 응답 완료 시 |
| **SessionStart / End** | 세션 시작 / 종료 시 |
| **UserPromptSubmit** | 사용자 입력 시 |

**생성 방법:** 프롬프트로 요청하거나 `settings.json`에 직접 작성
```
› 작업 완료되면 알림보내는 훅 만들어줘
```

**실제 사용 예시 — 응답 완료 시 알림:**
```json
{
  "hooks": {
    "Stop": [{
      "hooks": [{
        "type": "command",
        "command": "osascript -e '...알림...'"
      }]
    }]
  }
}
```

---

## Slide 23: Plugins (약 1분)

Skills + Hooks + MCP + Agents를 하나로 묶어 배포하는 패키지

**구조:**
```
my-plugin/
├── .claude-plugin/
│   └── plugin.json      ← 매니페스트 (필수)
├── skills/              ← 스킬들
├── agents/              ← 에이전트들
├── hooks/               ← 훅 설정
└── settings.json        ← 기본 설정
```

**plugin.json 예시:**
```json
{
  "name": "my-plugin",
  "description": "팀 공통 개발 도구",
  "version": "1.0.0",
  "author": { "name": "Your Name" }
}
```

**배포:** Git repo → 마켓플레이스 등록 → `/plugin install`로 설치

---

## Slide 24: 유용한 팁 (약 1분)

| 기능 | 설명 |
|------|------|
| **Escape 1번** | 현재 작업 중지 |
| **Escape 2번** | 입력 있으면 전체 삭제 / 없으면 rewind |
| **`/voice`** | 음성 입력 모드 (한국어 지원) |
| **이미지 붙여넣기** | 터미널에 스크린샷 직접 붙여넣기 |
| **`@파일경로`** | 파일을 컨텍스트에 참조 |
| **`!명령어`** | Claude 안에서 셸 명령어 실행 |
| **`/resume`** | 이전 세션 복귀 (30일간 보관) |
| **`/export`** | 대화 내보내기 |
| **`claude --continue`** | 방금 종료된 대화 재시작 |

---

## Slide 25: 데모 (약 5분)

**실제 워크플로우 시연**

1. 지라 티켓 전달
2. 관련 프로젝트 탐색
3. 코드 레벨 분석
4. 코드 수정
5. PR 생성

> (라이브 데모 또는 녹화 영상)

---
