---
name: freebuff_review
description: Review code and tests with Freebuff CLI using Solar Pro 4, validate findings, and prepare fixes and a GitHub pull request. Use when asked for freebuff_review or a Freebuff/Solar Pro 4 code review. Honor review-only requests.
---

# Freebuff Review

Use Freebuff with Solar Pro 4 to independently review the repository and scope requested by the user. Validate its findings, fix confirmed issues, run relevant checks, and prepare a reviewable pull request unless the user requests review only. Stay within the requested scope; do not merge or deploy.

These instructions are agent-independent. Use the host assistant's available shell, terminal, Git, and GitHub tools. No Codex-specific API is required. The skill name and conversational invocation are `freebuff_review`; native invocation syntax depends on the host.

A plain `freebuff_review` request defaults to the current repository and the full review, validation, fix, test, and PR workflow. The user does not need to repeat setup or model-selection instructions. Honor explicit scope and review-only constraints.

## Prepare and install

- Read repository instructions and inspect Git status, the remote default branch, and existing pull requests. Preserve ongoing user changes and compare PR changes against the current base branch. Follow repository branch conventions; otherwise use a descriptive branch such as `freebuff_review/<topic>`.
- Check whether Freebuff is installed (`command -v freebuff` on POSIX shells, or the platform equivalent). **If missing, install it before continuing.** Check Node.js and npm, then run `npm install -g freebuff`. Verify installation with `freebuff --version` and `freebuff --help`. Avoid reinstalling or upgrading an existing installation unnecessarily.
- If Node.js/npm is missing, inspect the operating system and available package manager and install a supported Node.js LTS release with npm, then install Freebuff. Follow the host's actual permission requirements. If global npm installation fails due to permissions, preserve existing configuration and use a user-owned npm prefix. Add its bin directory to the current session's PATH and explain any persistent setup needed. Do not default to `sudo` or changing system-directory permissions. If installation cannot be completed, report the failed command, cause, and required user action.
- Verify the installed CLI's supported options. Do not assume a `freebuff models` command or a model flag exists.
- Check authentication and, when needed, guide the user through `freebuff login` and browser authentication. Do not print credentials or include them in reports. Respect host permission denials and report the required action without bypassing them.

## Select the default model and run

- **Select Solar Pro 4 (`upstage/solar-pro4`) as the default model.** Launch `freebuff --cwd <repo>` and use the interactive `/model` selector when supported by the installed version.
- Verify that the default selection is saved. For versions using `~/.config/manicode/settings.json`, set only `freebuffModel` to `upstage/solar-pro4`, preserving every other JSON field and existing user setting. If the installed version uses a different configuration mechanism, inspect its actual help or implementation and use that supported mechanism.
- Reopen the CLI when necessary to confirm the default persists. Confirm the actual **Solar Pro 4** display in the running CLI as well as the saved setting. If the model is unavailable or the service switches to another model, report the condition instead of silently continuing. Never label another model's output as a Solar Pro 4 review.
- Keep an interactive PTY session open. If supported by the terminal, bracketed paste (`ESC[200~`, prompt text, `ESC[201~`) followed by Enter can submit the prompt. Some screens require a separate Enter after pasting; confirm actual submission. Summarize ANSI output and retry animations, and restrict log inspection to the current task's session.
- Resolve TLS download errors using the operating system's trusted certificate authorities. For compatible Node.js launchers, `NODE_OPTIONS=--use-system-ca` may help. On macOS, if a separate runtime requires a PEM bundle, export only public trusted certificates from the System/SystemRootCertificates keychains to a temporary PEM and use `NODE_EXTRA_CA_CERTS` where supported. Never disable certificate verification. If a known remedy fails, report the specific error instead of repeating identical retries.

## Request the independent review

Adapt this prompt to the requested scope:

> Use Solar Pro 4. Read relevant source and tests and independently review the current repository and requested changes for correctness bugs, performance opportunities, and test gaps. Report severity, exact file/line, triggering scenario, impact, suggested fix, and evidence. Distinguish verified issues from suspicions. Do not modify source, tests, config, git state, or dependencies. Write only FREEBUFF_CODE_REVIEW.md in the repository root. Exclude secrets, credentials, personal app data, generated artifacts and dependencies. Safe local tests are allowed; do not launch production services or use actual user accounts. State the model, reviewed commit/diff, scope, tests run and limitations. Finish the report before summarizing.

Find the project's actual test commands in repository instructions, build configuration, package scripts, and CI, and include relevant commands in the prompt. Do not import paths or test commands from another project. Preserve an existing review report before replacing it.

## Validate, fix, and prepare a pull request

- Confirm Freebuff's completion and the report's creation. Do not substitute your own review for Freebuff output when its model has not responded. If execution is blocked, report completed preparation and the remaining steps.
- Compare each finding with current code and tests. Explain findings that cannot be reproduced or are already resolved, and fix only validated issues. Add regression tests where necessary and run checks appropriate to the change. Verify performance claims with comparative measurements; do not present a local benchmark as whole-application performance.
- Report actual code and command results. Check that the report excludes secrets and personal data and that source changes match the requested scope. If no issues need fixing, a report-only PR may be appropriate when a PR was requested.
- Include existing user changes only when they belong to the requested scope and have been reviewed. Preserve unrelated work. Use an isolated worktree when needed without moving or discarding uncommitted user changes.
- For a review-only request, deliver the findings and report without modifying source or creating a PR. Otherwise commit verified fixes and the report, push the branch, and create a PR using `gh pr create --body-file <file>` or an available GitHub integration. Describe the problem, resulting behavior, validation status, test/performance results, and limitations. Follow the host's actual permission requirements.
- Verify the PR's base, head, state, and mergeability. If the host provides a native PR attachment feature, attach the URL; this is optional and does not require Codex. Deliver the PR link, key changes, test results, unresolved findings, and review report path. Do not automatically merge.
