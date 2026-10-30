# exam-kit

**시험·해커톤용 코딩 AI 준비를 링크 하나로.**

`sdd-tool`로 명세와 구현을 진행하고, `verification-before-completion`으로 검증 근거를 확인하며, Playwright로 실제 화면을 검사합니다. **설치는 Playwright CLI + 스킬 준비까지만 합니다. 실제 브라우저 검증은 앱 구현 후 필요한 때 수행하며, 그때 CLI가 복구되지 않으면 MCP를 추가합니다.**

## 한 번에 요청하기

시험의 **실제 과제 폴더를 연 코딩 AI**에게 아래 내용을 그대로 보내세요.

> https://github.com/JakeB-5/exam-kit 의 README.md와 AGENTS.md를 읽고 현재 과제 폴더에 exam-kit을 설치해줘. question.md가 있으면 먼저 읽고 원문을 보존해줘. sdd-tool, verification-before-completion, Playwright CLI와 스킬을 준비하고 sdd init으로 초기화해줘. 과제 루트 AGENTS.md는 실제 프로젝트의 목적·자료 기준·범위·핵심 동작·미확정 사항·검증/완료 기준 중심으로 작성해줘. 기존 프로젝트 규칙은 보존하고 SDD 관리 블록과 PREWRITE 상세 규칙은 docs/development-workflow.md로 분리해 루트에서는 필요할 때만 참조하게 해줘. 설치 중에는 Playwright 브라우저 실행·스모크/E2E·브라우저 다운로드·MCP 추가를 하지 마. 설치 파일과 과제 지침 반영 여부만 확인해줘. 이후 구현이 끝나면 요구사항 ID별 테스트 방법·실제 결과·증거 위치·미확인 사항을 기록하고 업무 담당자 검토 요청문을 준비·제안해줘. 가능한 작업은 직접 완료해줘.

더 짧게 요청해도 됩니다.

> https://github.com/JakeB-5/exam-kit 를 이 과제에 설치하고 question.md를 반영한 과제 루트 AGENTS.md를 작성해줘. 설치 중 브라우저 검증은 하지 마.

이 저장소는 **코딩 AI가 읽고 실행할 설치 절차와 미리 작성된 프로젝트 지침**입니다. 링크 자체가 설치를 실행하지는 않습니다. 파일·터미널을 사용할 수 있는 에이전트가 필요합니다. 패키지 설치나 보호된 스킬 폴더 쓰기가 제한되면 완료한 단계와 막힌 단계를 구분해 알려줍니다. 설치 완료는 브라우저 실행 성공을 의미하지 않습니다.

## 설치 후 달라지는 것

| 단계 | 기본 행동 |
| --- | --- |
| 구현 전 · Prewrite | 과제 자료를 읽고 확인된 요구사항·가설·질문을 구분합니다. 요구사항 ID와 인수 조건을 정합니다. |
| 구현 중 | 작은 단위로 구현하고 해당 요구사항을 실제로 확인합니다. |
| 구현 후 | 요구사항별 검증표와 증거를 남기고, 미확인 사항을 구분합니다. |
| 담당자 검토 | 구현 흐름·실제 사용 예시·검증 결과를 정리한 검토 요청문 작성을 자동으로 제안합니다. |

**예상 설치 위치**

```text
현재 과제/
├── AGENTS.md                   # 실제 프로젝트의 목적·범위·개발·완료 기준
├── CLAUDE.md                   # 필요한 경우 AGENTS.md 참조
├── .sdd/                       # 명세와 작업 관리
├── .agents/skills/             # Codex용 선택 스킬
├── .claude/skills/             # Claude Code용 선택 스킬
├── .exam-kit/                  # 로컬 도구, 설치 기록, 개인 증거 — 공개 제외
└── docs/
    ├── development-workflow.md # SDD·도구 사용법·상세 검증 절차
    ├── verification.md         # 요구사항별 실제 검증 결과
    └── review-request.md       # 담당자에게 전달할 검토 요청 초안
```

설치 절차는 전역 npm 설정, 기존 프로젝트 의존성, 다른 과제의 브라우저 세션을 바꾸지 않는 로컬 설치를 우선합니다. 재요청할 때는 기존 설치를 검사하고 필요한 부분만 복구합니다.

## 두 AGENTS.md의 역할

이 저장소의 **AGENTS.md는 설치자를 위한 안내**입니다. 생성되는 **과제 루트 AGENTS.md는 해당 프로젝트를 개발하기 위한 지침**입니다. 설치 안내나 SDD 매뉴얼을 루트에 합쳐 넣지 않습니다. `sdd init`의 관리 블록과 PREWRITE 전문은 `docs/development-workflow.md`에 보존하고 루트에는 과제에 필요한 판단 기준과 참조만 남깁니다.

## 설치 완료 확인 기준

- SDD: 실행 파일과 프로젝트 초기화 상태, 생성된 스킬을 확인합니다.
- 검증 스킬: 현재 에이전트가 읽을 수 있는 위치의 `SKILL.md`를 확인합니다.
- 과제 지침: question.md/지정 자료를 기준으로 과제 목적·범위·정책·미확정 사항이 루트 AGENTS.md의 중심인지, 상세 도구 안내가 별도 문서로 분리됐는지 확인합니다. `sdd init`만으로 과제 내용을 자동 해석했다고 가정하지 않습니다.
- Playwright: 실행 파일 버전과 스킬 파일·보조 리소스만 확인합니다. 실제 브라우저 검증 상태는 `NOT_RUN`이며 설치 완료를 막지 않습니다.
- 설치 상태는 `installed` / `partial`로 보고합니다. 설치 중 가상 페이지·스모크/E2E·스크린샷·브라우저 다운로드·MCP 대체 검사는 하지 않습니다.
- 구현 후 필요한 실제 화면 검사와 MCP 대체 절차는 [BROWSER-CHECKS.md](BROWSER-CHECKS.md)를 따릅니다.

설치 상세 절차는 [AGENTS.md](AGENTS.md), 별도 개발 절차 문서에 병합할 규칙은 [PREWRITE.md](PREWRITE.md), 루트 작성 기준은 [프로젝트 지침 틀](templates/PROJECT-AGENTS.md)에 있습니다.

## 구현 후 자동으로 제안하는 내용

> 요구사항별 테스트 방법·실제 결과·증거 위치·미확인 사항을 정리했습니다. 이 자료로 구현 흐름과 실제 사용 예시를 담은 담당자 검토 요청문을 준비할 수 있습니다. 다음으로 미확인 항목을 더 검증하거나 담당자에게 확인할 질문을 정리하는 것을 제안합니다.

실제로 작성하거나 검사한 것만 완료라고 보고합니다. 검토 요청문은 초안으로 준비하며, 담당자에게 메시지를 전송하거나 서비스를 공개하는 행동은 별도 요청이 있을 때만 합니다.

## 템플릿

- [프로젝트 AGENTS 작성 기준](templates/PROJECT-AGENTS.md)
- [요구사항별 검증표](templates/VERIFICATION.md)
- [담당자 검토 요청문](templates/REVIEW-REQUEST.md)

## 기준 버전과 확인 범위

기본 절차는 Node.js **22.20 이상**(24 LTS 권장), npm, Git을 기준으로 합니다. `skills` 설치 도구의 요구 버전까지 포함한 기준입니다.

| 구성 | 문서 기준 버전 |
| --- | --- |
| sdd-tool | 2.1.1 |
| @playwright/cli | 0.1.22 |
| skills | 1.7.4 |
| @playwright/mcp · 대체 경로 | 0.0.83 |

버전별 실제 확인 결과는 [CHECKS.md](CHECKS.md)에 기록합니다. 시험 도중 무조건 최신 버전으로 갱신하지 않습니다. Cofa 실행 환경에서의 지원 여부를 보증하는 저장소는 아닙니다.

## 원본 도구

- [SDD Tool](https://github.com/JakeB-5/sdd-tool) · [사용 문서](https://jakeb-5.github.io/sdd-tool/ko/)
- [Verification Before Completion](https://github.com/obra/superpowers/tree/main/skills/verification-before-completion)
- [Playwright CLI와 스킬](https://github.com/microsoft/playwright-cli)
- [Playwright MCP](https://github.com/microsoft/playwright-mcp)
- [Skills 설치 도구](https://github.com/vercel-labs/skills)

외부 스킬의 원문을 이 저장소에 복제하지 않고 원본에서 선택 설치합니다. 시험 주최 측의 도구 사용 규칙을 따르는 독립적인 개인 준비 키트입니다.
