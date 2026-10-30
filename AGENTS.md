# exam-kit — 설치 요청을 받은 코딩 AI에게

사용자가 이 저장소로 준비를 요청하면 **대상 과제에 설치하고 설치 산출물을 확인하라. 명령어 목록만 전달하고 끝내지 않는다.** 사용자의 현재 요청과 시험 환경 규칙을 우선하며, 기존 작업을 보존한다.

## 1. 대상과 현재 상태 확인

1. 사용자가 작업 중인 **과제 루트**를 식별한다. 이 저장소의 체크아웃을 과제 폴더로 착각하지 않는다. 대상이 명확하면 재확인하지 않는다. 여러 과제가 있고 대상을 식별할 수 없을 때만 묻는다.
2. 과제 루트에 `question.md`가 있으면 **설치 전에 먼저 읽고 원문을 보존**한다. 업무 목적·성공 기준·범위·자료/정책·미확정 항목을 구분한다. 없으면 사용자의 요청과 지정한 과제 자료를 기준으로 한다. 기존 `AGENTS.md`, `CLAUDE.md`, 패키지 매니저, `.sdd`, `.agents/skills`, `.claude/skills`, MCP 설정을 읽는다. 기존 프로세스·모델·인증 설정을 유지한다.
3. Node.js 22.20+, npm, Git을 확인한다. 필요한 런타임이 없으면 환경에서 허용하는 사용자 영역 설치를 이용한다. 권한이나 네트워크가 실제로 막힌 경우에만 해결이 필요한 항목을 구체적으로 알린다.
4. 원격 문서는 `https://raw.githubusercontent.com/JakeB-5/exam-kit/main/`에서 읽거나 이 저장소를 별도 도구 폴더에 복제한다. 과제 파일을 이 공개 저장소에 커밋하거나 업로드하지 않는다.
5. 아래 로컬 도구 설치와 프로젝트 지침 생성은 이미 요청된 작업이다. **설치 중 Playwright 실제 동작 검증은 하지 않는다.** 같은 허가를 반복해서 요청하지 않는다. 다른 사용자 세션을 종료하거나 전역 설치를 교체할 필요가 생기면 로컬 방식으로 해결한다.

## 2. 프로젝트 로컬 도구 설치

작업 디렉터리는 **과제 루트**다. 우선 `.exam-kit/tools/` 아래에 설치한다. 기존 정상 설치가 있으면 버전과 동작을 확인하고 재사용한다.

```sh
npm install --prefix .exam-kit/tools --no-audit --no-fund sdd-tool@2.1.1 @playwright/cli@0.1.22 skills@1.7.4
```

이후 명령의 실행 파일은 `.exam-kit/tools/node_modules/.bin/`에 있다. 새 셸에서도 그 절대 경로를 사용하거나 **해당 명령의 PATH에만** 추가한다. 셸 프로필이나 npm의 전역 prefix를 변경하지 않는다. Windows라면 플랫폼에 맞는 `.cmd` 실행 파일을 사용한다.

`package.json`과 잠금파일은 `.exam-kit/tools/` 내부에만 생성한다. `.exam-kit/`, `.playwright-cli/` 등 생성된 개인 로그·런타임 파일은 대상 저장소의 기존 ignore 정책을 보존하면서 공개 제외한다. 설치한 스킬과 AGENTS 지침까지 무조건 숨기지는 않는다.

문서의 버전은 검증 기준이다. 기존 버전이 정상 동작하면 무조건 다운그레이드하지 않는다. 기준 버전을 구할 수 없거나 명령이 다르면 실제 `--help`와 원본 문서를 확인하고 선택한 버전·이유를 설치 기록에 남긴다.

## 3. SDD 초기화와 스킬

```sh
.exam-kit/tools/node_modules/.bin/sdd --version
.exam-kit/tools/node_modules/.bin/sdd init --help
.exam-kit/tools/node_modules/.bin/sdd init --skip-git-setup
```

- **새 SDD 프로젝트일 때만** 초기화한다. 기존 `.sdd/`가 있으면 명세를 보존하고 누락된 설정만 문서화된 방법으로 보완한다. `--force`로 덮어쓰지 않는다.
- 생성되는 SDD 지침이 기존 `AGENTS.md` 내용과 함께 남아 있는지 확인한다. `sdd init`은 기본 지침을 생성하는 명령이며 `question.md`를 자동으로 해석하는 기능으로 간주하지 않는다. 과제별 지침은 7절에서 별도로 작성한다.
- 보호된 `.agents`/`.codex` 쓰기가 막히면 필요한 초기화 명령만 허용되는 승인 경로로 처리한다. 실패 때 생긴 `.sdd`가 있으면 내용을 확인하고 기존 명세를 보존한다. 빈 중단 산출물은 별도 보관한 뒤 재시도할 수 있지만 무조건 `--force`로 덮어쓰지 않는다.
- `.agents/skills/sdd-*/SKILL.md`와 `.claude/skills/sdd-*/SKILL.md`를 확인한다. 현재 에이전트에 맞는 경로가 실제로 존재해야 한다.
- `sdd validate`는 명세 검사이며, 제품의 기능 테스트를 대신하지 않는다. `sdd sync`의 연결 검사도 실행 성공의 증거로 바꾸지 않는다.
- Git hook·CI 추가나 자동 커밋·푸시는 설치 범위에 포함하지 않는다. 사용자가 별도로 요청하면 그때 수행한다.

## 4. 완료 전 검증 스킬 — 하나만 선택 설치

현재 사용하는 에이전트를 `--agent`에 지정한다. 아래는 Codex와 Claude Code를 함께 준비하는 예다. 다른 에이전트라면 `skills --help`에 나온 실제 식별자를 사용한다.

```sh
DISABLE_TELEMETRY=1 .exam-kit/tools/node_modules/.bin/skills add https://github.com/obra/superpowers --skill verification-before-completion --agent codex claude-code --copy -y
```

`superpowers` 전체를 설치하지 않는다. `--global`, `--all`도 사용하지 않는다. 같은 이름의 기존 사용자 수정 스킬은 덮어쓰지 말고 비교한 뒤 재사용하거나 충돌 내용을 알린다. 설치된 `SKILL.md`의 실제 위치와 내용을 확인한다. 새 스킬이 현재 대화에 자동으로 인식되지 않으면 경로로 직접 읽어 적용하고, 재로드가 필요한지는 별도로 알린다.

## 5. Playwright CLI + 스킬 설치

```sh
.exam-kit/tools/node_modules/.bin/playwright-cli --version
.exam-kit/tools/node_modules/.bin/playwright-cli install --help
```

현재 에이전트에 맞는 **프로젝트 범위** 스킬을 설치한다. 로컬 도움말이 지원하면 Codex는 `playwright-cli install --skills=agents`, Claude Code는 `playwright-cli install --skills`를 사용한다. Codex 경로를 지원하지 않으면 공식 스킬만 선택 설치한다.

```sh
DISABLE_TELEMETRY=1 .exam-kit/tools/node_modules/.bin/skills add https://github.com/microsoft/playwright-cli --skill playwright-cli --agent codex --copy -y
```

버전 출력과 실제 `SKILL.md`·보조 리소스의 존재를 확인한다. 브라우저 실행 성공 여부는 설치 완료 조건이 아니다.

### 설치 단계의 경계

- 가상 테스트 페이지나 서버를 만들지 않는다. 브라우저 열기·입력·클릭·스크린샷·E2E·스모크 테스트를 실행하지 않는다.
- 사용하지도 않은 브라우저의 다운로드·시스템 의존성 복구·권한 확대를 설치의 선행 조건으로 만들지 않는다.
- 브라우저를 검사하지 않았다는 이유로 MCP를 추가하거나 등록하지 않는다.
- 설치 완료와 런타임 동작 확인을 구분한다. 현재 브라우저 검증 상태는 **NOT_RUN(설치 범위 밖)**이다.

## 6. 실제 브라우저 검사는 구현 후 필요한 때

이 절은 **설치 중 실행할 단계가 아니다.** 앱 구현 후 UI 검증이 필요하거나 사용자가 별도로 브라우저 동작 검사를 요청했을 때만 [BROWSER-CHECKS.md](BROWSER-CHECKS.md)를 따른다.

그때 CLI로 실제 대상 앱을 검사하고, 확인된 실행 문제를 복구해도 사용할 수 없는 경우에만 Playwright MCP를 대체 경로로 추가한다. 단순한 미검사 상태나 앱 자체의 테스트 실패를 CLI 장애로 취급하지 않는다.

## 7. 과제 루트 AGENTS.md 작성 및 Prewrite 병합

1. `question.md` 또는 지정 과제 자료를 근거로, 과제 루트 `AGENTS.md`에 **과제별 지침**을 작성한다. 목적/성공 기준, 자료의 기준과 읽기 순서, 첫 버전·제외 범위, 중요한 정책/상태 보존 조건, 미확정 사항, 검증·보고 원칙을 포함한다. 도구의 설치 안내를 그대로 복사하는 것으로 대신하지 않는다. 상세 사실은 원문/SDD를 참조하고 설계 제안을 확정 요구사항으로 바꾸지 않는다. 과제별 지침은 `sdd:begin/end`와 `exam-kit:begin/end` 관리 블록 밖에 둔다.
2. 이 저장소의 `PREWRITE.md`에서 `<!-- exam-kit:begin -->`부터 `<!-- exam-kit:end -->`까지를 과제 루트의 `AGENTS.md`에 추가한다. 기존 내용과 SDD 블록은 그대로 보존한다.
3. 이미 exam-kit 블록이 있으면 중복 추가하지 않는다. 같은 내용이면 유지한다. 달라졌으면 사용자의 수정 여부를 확인하고 필요한 변경만 병합한다.
4. Claude Code에서 `AGENTS.md` 참조가 없으면 기존 `CLAUDE.md`를 보존하면서 `@AGENTS.md`를 추가한다. 다른 호스트는 지원되는 프로젝트 지침 위치에 참조 또는 같은 블록을 반영한다. 폴더에 저장한 것과 호스트가 실제 읽는 것은 구분한다.
5. 템플릿 `templates/VERIFICATION.md`, `templates/REVIEW-REQUEST.md`를 `.exam-kit/templates/`에 둔다. 작업 결과 문서가 이미 있으면 템플릿으로 덮어쓰지 않는다.
6. 작성 후 `question.md`/지정 자료와 대조한다. 원문 보존, 과제 목적·범위·정책·미정 사항 반영, SDD 블록과 exam-kit 블록의 중복 없음, 실제 링크/스킬 경로를 확인한다. 명세·계획 준비도 요청됐다면 과제별 SDD 문서를 작성하고 `sdd validate --strict`로 형식을 검사한다. 초기화 성공을 과제 의미 반영이나 제품 기능 통과로 바꾸지 않는다.
7. 사용자가 설치·준비까지만 요청했다면 앱 구현·앱 테스트·실제 AI 호출을 시작하지 않는다. 향후 요구사항 검증표는 미실행 항목을 NOT_RUN으로 두고 실제 실행 사례를 꾸며내지 않는다.

## 8. 설치 결과 보고

`.exam-kit/setup-report.md`에 날짜, 과제 루트, 설치 버전, 실제 실행 파일·스킬 경로, 설치 산출물 확인 결과, 과제 지침 반영 내용, 증거 위치, 미해결 제한을 기록한다. 비밀값과 인증 내용을 넣지 않는다.

최종 응답은 아래를 짧게 알려준다.

- 설치 상태: `installed` / `partial`. 도구·필요 스킬·SDD/과제 지침이 준비됐으면 `installed`, 필요한 설치 산출물이 빠졌으면 `partial`
- 설치한 세 구성요소와 과제 지침 병합 여부
- Playwright 런타임 검증: `NOT_RUN`(설치 범위 밖). 이전 검사 이력이 있어도 현재 설치의 필수 조건으로 삼거나 자동 재검사하지 않는다
- 재로드 등 필요한 사용자 행동이 있으면 그 행동만
- 이후 구현 완료 시 요구사항 검증표와 담당자 검토 요청문을 자동으로 제안한다는 점

필요한 도구·스킬·과제 지침이 준비되지 않았다면 나머지 성공 단계는 보존하고 `partial`로 설명한다. 브라우저를 실행하지 않았다는 이유만으로 설치를 `partial`로 표시하지 않는다. 모든 설치를 처음부터 반복하지 않는다.
