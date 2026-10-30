# exam-kit — 설치 요청을 받은 코딩 AI에게

사용자가 이 저장소로 준비를 요청하면 **대상 과제에 설치하고 검증하라. 명령어 목록만 전달하고 끝내지 않는다.** 사용자의 현재 요청과 시험 환경 규칙을 우선하며, 기존 작업을 보존한다.

## 1. 대상과 현재 상태 확인

1. 사용자가 작업 중인 **과제 루트**를 식별한다. 이 저장소의 체크아웃을 과제 폴더로 착각하지 않는다. 대상이 명확하면 재확인하지 않는다. 여러 과제가 있고 대상을 식별할 수 없을 때만 묻는다.
2. 기존 `AGENTS.md`, `CLAUDE.md`, 패키지 매니저, `.sdd`, `.agents/skills`, `.claude/skills`, MCP 설정을 읽는다. 기존 프로세스·모델·인증 설정을 유지한다.
3. Node.js 22.20+, npm, Git을 확인한다. 필요한 런타임이 없으면 환경에서 허용하는 사용자 영역 설치를 이용한다. 권한이나 네트워크가 실제로 막힌 경우에만 해결이 필요한 항목을 구체적으로 알린다.
4. 원격 문서는 `https://raw.githubusercontent.com/JakeB-5/exam-kit/main/`에서 읽거나 이 저장소를 별도 도구 폴더에 복제한다. 과제 파일을 이 공개 저장소에 커밋하거나 업로드하지 않는다.
5. 아래 설치·스모크 테스트는 이미 요청된 작업이다. 같은 허가를 반복해서 요청하지 않는다. 다른 사용자 세션을 종료하거나 전역 설치를 교체할 필요가 생기면 로컬 방식으로 해결한다.

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
- 생성되는 SDD 지침이 기존 `AGENTS.md` 내용과 함께 남아 있는지 확인한다.
- `.agents/skills/sdd-*/SKILL.md`와 `.claude/skills/sdd-*/SKILL.md`를 확인한다. 현재 에이전트에 맞는 경로가 실제로 존재해야 한다.
- `sdd validate`는 명세 검사이며, 제품의 기능 테스트를 대신하지 않는다. `sdd sync`의 연결 검사도 실행 성공의 증거로 바꾸지 않는다.
- Git hook·CI 추가나 자동 커밋·푸시는 설치 범위에 포함하지 않는다. 사용자가 별도로 요청하면 그때 수행한다.

## 4. 완료 전 검증 스킬 — 하나만 선택 설치

현재 사용하는 에이전트를 `--agent`에 지정한다. 아래는 Codex와 Claude Code를 함께 준비하는 예다. 다른 에이전트라면 `skills --help`에 나온 실제 식별자를 사용한다.

```sh
DISABLE_TELEMETRY=1 .exam-kit/tools/node_modules/.bin/skills add https://github.com/obra/superpowers --skill verification-before-completion --agent codex claude-code --copy -y
```

`superpowers` 전체를 설치하지 않는다. `--global`, `--all`도 사용하지 않는다. 같은 이름의 기존 사용자 수정 스킬은 덮어쓰지 말고 비교한 뒤 재사용하거나 충돌 내용을 알린다. 설치된 `SKILL.md`의 실제 위치와 내용을 확인한다. 새 스킬이 현재 대화에 자동으로 인식되지 않으면 경로로 직접 읽어 적용하고, 재로드가 필요한지는 별도로 알린다.

## 5. Playwright CLI + 스킬 우선

```sh
.exam-kit/tools/node_modules/.bin/playwright-cli --version
.exam-kit/tools/node_modules/.bin/playwright-cli install --skills
```

기준 버전의 `install --skills`는 `.claude/skills/playwright-cli`를 만든다. Codex 등 다른 에이전트용 경로가 없으면 공식 스킬을 선택 설치한다.

```sh
DISABLE_TELEMETRY=1 .exam-kit/tools/node_modules/.bin/skills add https://github.com/microsoft/playwright-cli --skill playwright-cli --agent codex --copy -y
```

설치 경로·보조 리소스를 확인하고 현재 에이전트에 맞는 스킬을 읽는다. CLI와 스킬을 준비했더라도 **실제 브라우저 검사가 통과하기 전에는 브라우저 준비 완료라고 하지 않는다.**

### 실제 동작 검사

1. 이 작업만을 위한 고유한 이름의 세션(`-s=exam-kit-<고유값>`)을 사용한다. 다른 브라우저나 세션을 닫지 않는다.
2. 별도 임시 폴더의 작은 HTML 페이지를 빈 loopback 포트에서 제공한다. 입력란, 버튼, 결과 표시가 있는 자가 테스트면 충분하다. 과제의 앱 코드를 변경해서 스모크 테스트를 만들지 않는다.
3. 기본 headless 방식으로 열고, 화면 상태를 확인한 뒤 입력·클릭을 실행한다. 입력한 값이 실제 결과에 나타나는지 검증한다.
4. 그 세션의 스크린샷이나 스냅샷과 명령 결과를 `.exam-kit/evidence/setup/`에 저장한다. 이는 **도구 확인용 가상 페이지**의 결과이며 과제의 E2E 통과라고 보고하지 않는다.
5. 테스트 페이지 서버와 이 작업에서 연 브라우저 세션만 종료한다. `close-all`, `kill-all`로 다른 세션을 정리하지 않는다.

브라우저 실행 파일이 없으면 실제 `playwright-cli --help install-browser` 안내에 따라 사용자 영역에 필요한 브라우저를 설치하고 재검사한다. 표시 장치가 없으면 headless를 사용한다. PATH·실행 권한·브라우저 의존성 등 확인된 원인을 한 번 복구한 뒤 재시도한다. 애플리케이션 자체의 테스트 실패를 CLI 장애로 오해하지 않는다.

## 6. CLI가 복구되지 않을 때만 MCP 추가

CLI 실패 원인과 복구 시도를 기록한 후 MCP 대체 경로로 넘어간다. CLI가 정상 작동하면 MCP를 중복 추가하지 않는다. MCP도 같은 브라우저 실행 제약을 받을 수 있으므로 반드시 실제 도구 호출로 확인한다.

```sh
npm install --prefix .exam-kit/tools --no-audit --no-fund @playwright/mcp@0.0.83
```

현재 호스트의 MCP 등록 기능을 사용한다. 등록 기능이 없으면 실제 호스트가 읽는 **프로젝트 범위 설정**을 확인하고 기존 서버 항목과 병합한다. 아래 형식은 `mcpServers`를 지원하는 호스트용 예시이며 모든 제품의 설정 형식이 같지는 않다.

```json
{
  "mcpServers": {
    "exam-playwright": {
      "command": "<NODE_ABSOLUTE_PATH>",
      "args": [
        "<PROJECT_ABSOLUTE_PATH>/.exam-kit/tools/node_modules/@playwright/mcp/cli.js",
        "--headless",
        "--isolated"
      ]
    }
  }
}
```

- 실제 절대 경로로 치환한다. 경로를 추측하거나 예시 placeholder 그대로 적용하지 않는다.
- 원격 URL만 받는 호스트라면 지원되는 전송 방식과 브라우저에서 과제 앱에 접근 가능한 네트워크를 먼저 확인한다. 본인 PC의 `localhost`를 원격 실행 환경 주소로 넘기지 않는다.
- 호스트에 설정을 쓸 권한이 없으면 붙여넣을 수 있는 정확한 설정을 준비하고, 사람이 해야 하는 연결 단계만 요청한다.
- MCP 도구 목록 조회, 페이지 열기, 입력·클릭·결과 확인이 실제로 통과해야 `mcp-ready`다. 설정 작성만 끝났다면 `needs-host-action` 또는 `unverified`로 보고한다.

## 7. Prewrite 지침을 과제에 병합

1. 이 저장소의 `PREWRITE.md`에서 `<!-- exam-kit:begin -->`부터 `<!-- exam-kit:end -->`까지를 과제 루트의 `AGENTS.md`에 추가한다. 기존 내용과 SDD 블록은 그대로 보존한다.
2. 이미 exam-kit 블록이 있으면 중복 추가하지 않는다. 같은 내용이면 유지한다. 달라졌으면 사용자의 수정 여부를 확인하고 필요한 변경만 병합한다.
3. Claude Code에서 `AGENTS.md` 참조가 없으면 기존 `CLAUDE.md`를 보존하면서 `@AGENTS.md`를 추가한다. 다른 호스트는 지원되는 프로젝트 지침 위치에 참조 또는 같은 블록을 반영한다. 폴더에 저장한 것과 호스트가 실제 읽는 것은 구분한다.
4. 템플릿 `templates/VERIFICATION.md`, `templates/REVIEW-REQUEST.md`를 `.exam-kit/templates/`에 둔다. 작업 결과 문서가 이미 있으면 템플릿으로 덮어쓰지 않는다.

## 8. 설치 결과 보고

`.exam-kit/setup-report.md`에 날짜, 과제 루트, 설치 버전, 실제 실행 파일·스킬 경로, 선택한 브라우저 경로(CLI/MCP), 수행한 검사, 증거 위치, 미해결 제한을 기록한다. 비밀값과 인증 내용을 넣지 않는다.

최종 응답은 아래를 짧게 알려준다.

- `ready-cli` / `ready-mcp` / `partial` 중 실제 상태
- 설치한 세 구성요소와 과제 지침 병합 여부
- 실제 브라우저 검사 결과와 증거 위치
- 재로드 등 필요한 사용자 행동이 있으면 그 행동만
- 이후 구현 완료 시 요구사항 검증표와 담당자 검토 요청문을 자동으로 제안한다는 점

브라우저나 스킬이 준비되지 않았다면 나머지 성공 단계는 보존하고 `partial`로 설명한다. 모든 설치를 처음부터 반복하지 않는다.
