# Freebuff Review

Freebuff CLI의 **Solar Pro 4**로 코드를 리뷰하고, 발견 사항을 검증·수정·테스트한 뒤 GitHub PR을 만드는 Codex 스킬입니다. 리뷰만 요청하면 수정과 PR 생성은 생략합니다.

## 설치

저장소를 복제하고 스킬 폴더를 Codex 스킬 디렉터리에 복사합니다. 기존 `freebuff-review`가 있으면 먼저 내용을 확인하세요.

```bash
git clone https://github.com/hunkim/freebuff_review.git
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
cp -R freebuff_review/skills/freebuff-review "${CODEX_HOME:-$HOME/.codex}/skills/"
```

새 Codex 세션에서 `$freebuff-review`를 호출합니다.

## 사용 예시

```text
$freebuff-review 이 저장소를 Solar Pro 4로 리뷰하고,
검증된 문제를 수정한 뒤 테스트하고 PR을 만들어줘.
```

```text
$freebuff-review 현재 변경 사항만 리뷰해줘. 수정과 PR 생성은 하지 마.
```

## 실행 흐름

1. 저장소 지침과 Git 상태, 리뷰 범위를 확인합니다.
2. **Freebuff가 없으면 설치합니다.** Node.js/npm이 없다면 환경에 맞는 설치부터 진행하고, `npm install -g freebuff` 후 설치 결과를 확인합니다. 필요한 환경 권한은 해당 환경의 승인 절차를 따릅니다.
3. 필요한 경우 `freebuff login`으로 사용자 브라우저 인증을 안내합니다. **기본 모델을 Solar Pro 4(`upstage/solar-pro4`)로 선택·저장**하고, 실제 CLI 표시와 기본 선택 유지 여부를 확인합니다.
4. Freebuff가 `FREEBUFF_CODE_REVIEW.md`를 작성합니다.
5. 발견 사항을 코드와 테스트로 검증하고, 요청 범위에 따라 수정·테스트·PR 생성을 진행합니다.

Freebuff CLI, Node.js/npm, Git이 필요하며, PR 생성에는 인증된 GitHub CLI(`gh`) 또는 사용 가능한 GitHub 도구가 필요합니다. Freebuff 인증은 사용자 계정으로 완료해야 합니다. CLI 옵션과 모델 제공 여부는 실행 시 확인하며, 다른 모델의 결과를 Solar Pro 4 리뷰로 표시하지 않습니다.

## 구성

- [`skills/freebuff-review/SKILL.md`](skills/freebuff-review/SKILL.md): 리뷰, 설치, 검증 및 PR 워크플로
- [`skills/freebuff-review/agents/openai.yaml`](skills/freebuff-review/agents/openai.yaml): Codex UI 메타데이터

이 저장소는 스킬 지침을 제공합니다. Freebuff나 Solar Pro 4의 소스 코드는 포함하지 않습니다. 기존 `freebuff_sp4_review` 스킬에서 프로젝트 전용 지침을 제거해 공개용으로 정리했습니다.

## License

[MIT](LICENSE)
