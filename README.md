# Freebuff Review

An agent-independent skill for reviewing code with **Freebuff CLI and Solar Pro 4**, validating findings, fixing confirmed issues, running tests, and preparing a GitHub pull request. Review-only requests skip fixes and PR creation.

The core instructions use ordinary terminal, Git, and GitHub capabilities and can be adapted by Codex, Claude Code, and other coding assistants that support local skills or reusable instructions. The assistant installs Freebuff if it is missing and selects **Solar Pro 4 (`upstage/solar-pro4`) as the default model**.

## Give this repository URL to your assistant

Use the following two messages in your coding assistant. The skill name, folder, and conversational invocation are all `freebuff_review`.

### 한국어

**1. 스킬 만들기**

```text
https://github.com/hunkim/freebuff_review 참고해서 freebuff_review 스킬 만들어줘.
```

**2. 바로 실행하기**

```text
freebuff_review 해줘.
```

리뷰만 원하면: `freebuff_review 해줘. 현재 변경 사항만 리뷰하고 수정이나 PR 생성은 하지 마.`

### English

**1. Create the skill**

```text
Create a freebuff_review skill from https://github.com/hunkim/freebuff_review.
```

**2. Run it immediately**

```text
Run freebuff_review.
```

For review only: `Run freebuff_review on the current changes. Do not modify source or create a PR.`

### 日本語

**1. スキルを作成する**

```text
https://github.com/hunkim/freebuff_review を参考に freebuff_review スキルを作成してください。
```

**2. すぐに実行する**

```text
freebuff_review を実行してください。
```

レビューのみの場合: `freebuff_review を実行してください。現在の変更だけをレビューし、ソースの修正やPRの作成はしないでください。`

## Instructions for the installing assistant

A request to create this skill from the repository URL includes the setup below. Carry it out without asking the user to repeat file names, installation paths, or model settings. A plain `freebuff_review` request runs the full workflow on the current repository: review, validate, fix, test, and create a PR. Honor any narrower scope or review-only instruction.

1. Fetch and read [`skills/freebuff_review/SKILL.md`](skills/freebuff_review/SKILL.md) in full. If the repository page does not expose file contents, use the [raw skill file](https://raw.githubusercontent.com/hunkim/freebuff_review/main/skills/freebuff_review/SKILL.md) or clone the repository.
2. Detect the host assistant's supported skill format, installation location, and discovery rules. Install the core `SKILL.md` there, preserving its workflow and the user's existing skills. Keep the requested skill name `freebuff_review`. If the host requires a different native identifier, explain that constraint and keep `freebuff_review` as the conversational invocation. Do not assume every assistant uses Codex paths or invocation syntax.
3. The [`agents/openai.yaml`](skills/freebuff_review/agents/openai.yaml) file is optional Codex UI metadata. Other assistants can ignore it. No host-specific API is required by the workflow.
4. Check and install Freebuff and any missing Node.js/npm prerequisites, then authenticate when necessary and select/save Solar Pro 4 as the default. Follow the host's actual permission requirements. Browser authentication may require the user.
5. Confirm the installed skill and explain the host's native invocation syntax. If it needs a new session to discover installed skills, say so. When asked to run immediately, read and apply the installed instructions in the current session if the host permits it.
6. If the host cannot persist custom skills, explain the limitation and apply the core instructions directly to the requested review using its available tools. Do not claim a persistent skill was installed.

## Workflow

1. Read repository instructions, Git state, and the requested scope.
2. Install missing Freebuff and prerequisites, and verify the installed CLI.
3. Authenticate when needed; select and save Solar Pro 4 as the default and verify the running CLI's model display.
4. Have Freebuff write `FREEBUFF_CODE_REVIEW.md`.
5. Validate findings and, within the requested scope, fix issues, run checks, and create a PR.

The assistant needs local terminal access to run Freebuff, Node.js/npm, and Git. PR creation also needs an authenticated GitHub CLI (`gh`) or another available GitHub integration. Model availability and CLI options are checked at runtime. Another model's output must not be reported as a Solar Pro 4 review.

## Files

- [`skills/freebuff_review/SKILL.md`](skills/freebuff_review/SKILL.md): portable installation, model selection, review, validation, and PR instructions.
- [`skills/freebuff_review/agents/openai.yaml`](skills/freebuff_review/agents/openai.yaml): optional Codex UI metadata.

This repository contains skill instructions, not the Freebuff or Solar Pro 4 source code.

## License

[MIT](LICENSE)
