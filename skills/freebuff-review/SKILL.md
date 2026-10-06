---
name: freebuff-review
description: Freebuff CLI의 Solar Pro 4로 코드와 테스트를 리뷰하고, 발견 사항을 검증·수정·테스트한 뒤 GitHub pull request까지 생성한다. $freebuff-review 또는 Freebuff/Solar Pro 4 코드 리뷰를 요청할 때 사용한다.
---

# Freebuff Solar Pro 4 review

사용자가 지정한 저장소와 범위를 Solar Pro 4로 리뷰하고, 검증된 발견 사항을 처리해 리뷰 가능한 PR을 만든다. 명시적으로 리뷰만 요청하면 수정·PR 단계를 생략한다. 사용자 요청 범위 밖의 변경이나 PR 병합·배포는 하지 않는다.

## 준비와 실행

- 저장소 지침, git 상태, 원격 main/default branch 및 기존 PR을 확인한다. 진행 중인 사용자 변경을 보존하고, PR 생성 시 최신 기준 브랜치와 비교한다. 새 브랜치는 기본적으로 `codex/` 접두사를 쓴다.
- `command -v freebuff`로 설치 여부를 확인한다. **미설치라면 설치까지 수행한 뒤 리뷰를 계속한다.** `command -v node`와 `command -v npm`으로 선행 조건을 확인하고 `npm install -g freebuff`를 실행한다. 이어서 `command -v freebuff`, `freebuff --version`, `freebuff --help`로 설치 성공과 실제 지원 옵션을 확인한다. 이미 설치되어 있으면 불필요하게 재설치하거나 업그레이드하지 않는다.
- Node.js/npm이 없으면 운영체제와 기존 패키지 관리자를 확인해 지원되는 Node.js LTS와 npm을 설치한 뒤 Freebuff 설치를 이어간다. 설치에 실제로 필요한 환경 권한은 환경의 승인 절차를 따른다. 전역 npm 설치가 권한 오류로 실패하면 기존 설정을 보존하고 사용자 소유 npm prefix를 사용한다. `sudo`나 시스템 디렉터리 권한 변경을 기본 해결책으로 삼지 않는다. 현재 세션 PATH에 prefix의 bin을 추가하고, 이후 세션에 필요한 설정을 안내한다. 설치를 완료할 수 없으면 실패 명령과 원인, 필요한 사용자 행동을 보고하고 리뷰 완료로 표시하지 않는다.
- 버전마다 옵션이 바뀌므로 `freebuff models`나 모델 플래그가 존재한다고 가정하지 않는다.
- **기본 모델은 Solar Pro 4(`upstage/solar-pro4`)로 선택한다.** `freebuff --cwd <repo>`로 대화형 CLI를 실행하고 `/model` 선택기에서 Solar Pro 4를 선택한다. 기본값 저장 여부를 확인하고, 실행 중인 버전이 `~/.config/manicode/settings.json`의 `freebuffModel`을 사용하는 경우 해당 값만 `upstage/solar-pro4`로 설정한다. 기존 JSON의 다른 필드와 사용자 설정은 보존한다. 해당 버전이 다른 설정 위치나 방식을 사용하면 실제 설치와 도움말을 확인해 지원되는 방식으로 저장한다.
- 설정을 저장한 뒤 필요하면 CLI를 다시 열어 기본 선택이 유지되는지 확인한다. 설정 값뿐 아니라 실제 CLI 하단의 **Solar Pro 4** 표시도 확인한다. 모델이 제공되지 않거나 다른 모델로 자동 대체되면 임의로 리뷰를 계속하지 말고 상태를 보고한다. 다른 모델의 결과를 Solar Pro 4 리뷰로 표시하지 않는다.
- 인증 여부를 확인하고 필요 시 사용자가 `freebuff login`과 브라우저 인증을 완료하도록 안내한다. 인증 정보나 토큰은 출력하거나 보고서에 담지 않는다. 자동 승인 검토가 로그인/서비스 접근을 거절하면 우회하지 말고 거절 이유와 필요한 사용자 행동을 알린다.
- TLS 다운로드 오류는 macOS 신뢰 인증서로 해결한다. Node 런처에는 `NODE_OPTIONS=--use-system-ca`를 사용할 수 있다. 네이티브 런타임에도 별도 인증서가 필요하면 System/SystemRootCertificates 키체인의 **공개 인증서**를 임시 PEM에 내보내 `NODE_EXTRA_CA_CERTS`로 지정한다. 인증서 검증을 끄지 않는다. 알려진 해결법 적용 후에도 연결이 계속 실패하면 동일 재시도를 반복하지 말고 구체적 오류를 보고한다.
- 대화형 실행은 PTY를 유지한다. 프롬프트는 bracketed paste(`ESC[200~`, 텍스트, `ESC[201~`) 후 Enter로 제출할 수 있다. 일부 화면은 붙여넣기 후 Enter를 따로 한 번 더 보내야 하므로 실제 제출 여부를 확인한다. 화면의 ANSI 제어 문자·재시도 애니메이션은 압축해 보고하고, 로그 확인은 현재 작업 세션에 한정한다.

## 리뷰 프롬프트

범위에 맞게 다음 요건을 전달한다:

> Use Solar Pro 4. Read relevant source and tests and independently review the current repository and requested changes for correctness bugs, performance opportunities, and test gaps. Report severity, exact file/line, triggering scenario, impact, suggested fix, and evidence. Distinguish verified issues from suspicions. Do not modify source, tests, config, git state, or dependencies. Write only FREEBUFF_CODE_REVIEW.md in the repository root. Exclude secrets, credentials, personal app data, generated artifacts and dependencies. Safe local tests are allowed; do not launch production services or use actual user accounts. State the model, reviewed commit/diff, scope, tests run and limitations. Finish the report before summarizing.

저장소 지침, 빌드 설정, 패키지 스크립트와 CI에서 실제 프로젝트의 테스트 명령을 찾아 프롬프트에 추가한다. 다른 프로젝트의 경로나 테스트 명령을 가져오지 않는다.

## 검증, 수정, PR

- Freebuff의 완료 표시와 보고서 생성을 확인한다. 모델이 응답하지 않았는데 자체 리뷰를 Freebuff 결과로 대체하지 않는다. 실행이 막히면 완료한 준비 작업과 남은 단계만 보고한다.
- 보고서의 지적을 현재 코드·테스트와 대조한다. 재현 조건이 없거나 이미 해결된 사항은 보류/해결됨으로 설명하고, 검증된 문제만 수정한다. 필요한 회귀 테스트를 추가하고 변경 범위에 맞는 검사를 실행한다. 속도 개선은 비교 측정으로 확인하고 국소 벤치마크를 전체 앱 속도로 표현하지 않는다.
- 보고서에는 코드·명령의 실제 결과만 적는다. 비밀/사용자 데이터가 없고 소스 변경이 의도한 범위인지 확인한다. 검토 결과에 수정할 문제가 없으면 보고서와 검증 결과를 PR로 제공할 수 있다.
- 사용자 요청에 포함된 기존 변경도 리뷰했으면 PR에 포함할 수 있다. 관련 없는 사용자 작업은 별도 보존한다. 필요 시 독립 worktree를 사용하되 미커밋 사용자 변경을 옮기거나 버리지 않는다.
- 검증된 수정과 보고서를 커밋하고 브랜치를 push한 뒤 `gh pr create --body-file <file>` 등으로 PR을 생성한다. PR 본문은 문제와 바뀐 동작, 항목별 검증 상태, 테스트/성능 결과, 제한 사항을 설명한다. 별도의 권한 질문은 실제로 필요할 때만 한다.
- PR URL을 현재 Codex 작업에 attach_artifact 도구가 있으면 연결한다. PR의 base/head/state와 충돌 상태를 확인하고, 최종 답변에 PR 링크, 중요한 수정, 테스트 결과, 미해결 사항과 리뷰 보고서 경로를 제공한다. PR을 자동 병합하지 않는다.
