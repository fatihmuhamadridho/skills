# Codex global workflow rules

These rules apply to Codex and any agent that reads `.codex/AGENTS.md`.

- Always prefix shell commands with `rtk`.
- In chained commands, prefix each command component with `rtk` too.
- Prefer RTK-native subcommands when available: use `rtk grep` instead of `rtk rg`, `rtk read` instead of raw file reads, and `rtk find`/`rtk tree` for filesystem inspection. Use `rtk ls` only if a native `ls` binary is actually available on PATH.
- For file reads, keep plain reads context-preserving by default: use `rtk read -l minimal` for ordinary reads, `rtk read -l aggressive --max-lines <N>` only when the task is explicitly partial, and `--tail-lines <N>` for end-of-file checks.
- Treat these as first-class rewrites for repeated Codex workflows: `Get-Content` or `cat` -> `rtk read -l minimal`, `Get-Content ... -TotalCount N` -> `rtk read -l aggressive --max-lines N`, `Get-Content ... -Tail N` -> `rtk read -l aggressive --tail-lines N`, `rg --files [path]` -> `rtk find [path]`, and bare `Get-ChildItem -Force [path]` -> `rtk find [path]`.
- Route the remaining frequent families through RTK too: `rg <pattern>` -> `rtk grep`, `Select-String -Path ... -Pattern ...` or `Select-String -Pattern ... -Path ...` -> `rtk grep`, `Select-String ... -Context A,B` -> `rtk grep <pattern> <path> -B A -A B`, `Get-ChildItem <path> | Where-Object { $_.Name -like 'foo*' }` -> `rtk find <path> -name 'foo*'`, `adb ...` -> `rtk summary adb ...`, `Get-CimInstance`/`Get-PSDrive`/`Get-Process` -> `rtk summary C:\tools\rtk\psinspect.cmd <topic>`, `git status|diff|commit|push` -> `rtk git ...`, and `pnpm ...` -> `rtk pnpm ...`.
- Use `rtk proxy <cmd>` only when a command must be run without filtering for compatibility/debugging.
- Do not use `rtk proxy` for ordinary inspection, status, or filesystem commands when `rtk` can run them directly.
- Treat `rtk proxy` as a last-resort exception; prefer `rtk` first and switch to `proxy` only after a real need is confirmed.
- Do not emit `rtk proxy powershell -NoProfile -Command "Get-Content ..."` or `rtk proxy powershell -NoProfile -Command "Select-String ..."` for routine code/file inspection. Rewrite them to RTK-native `read` or `grep` forms first.
- Do not force `rtk` onto unsupported shell launchers or built-ins such as `powershell`, `pwsh`, `cmd`, `where`, `echo`, `type`, or `Get-Command`; use RTK-native subcommands first, and if no native path exists, let the shell handle the command natively instead of creating fallback noise.
- Prefer POSIX shell syntax in Git Bash / MSYS on Windows.
- Keep the RTK executable available at `/c/tools/rtk/rtk`.

@C:/Users/fatih/.codex/RTK.md

## Tooling Preference

- Prefer `pnpm` over `npm` for Node.js package management and CLI installs when a Node-based tool is needed.
- Avoid `npm` unless there is no workable `pnpm` path for the task.
- For Python package installs, use `python -m pip` instead of relying on a bare `pip` command.

## Skill Routing Priority

- If the user request clearly matches an available skill, you must use that skill before defaulting to a generic direct-execution path.
- Treat a strong skill match as mandatory routing, not a soft preference, unless the user explicitly asks not to use the skill or the skill is unavailable/blocking.
- Before executing, map the request to candidate skills and choose the most specific applicable skill first.
- If a task matches a specific operational skill such as commit/push, code review, debugging, browser testing, Android automation, document editing, or spreadsheet generation, do not bypass the skill just because the task looks simple.
- If you intentionally do not use an obvious matching skill, state the reason explicitly in the working notes before proceeding.
- Keep `.codex/skills` as the primary skill source.
- If a `.codex` skill and a `.agents` skill both match, prefer `.codex` when it is more specific to the user's environment, workflow, app, repo, or repeated habits.
- If a `.agents` skill clearly matches the request and is not less specific than the available `.codex` option, use the `.agents` skill.
- Do not skip a relevant `.agents` skill just because a `.codex` skill exists in the same broad area.
- Prefer the most specific skill, not the most generic one.
- When multiple skills are complementary, use both in the minimal order that helps execution.
- For small or obvious tasks, do not force an extra generic `.agents` skill when it adds process overhead without improving execution.

Practical defaults:

- Prefer `.codex` for user-specific environment flows, local repo conventions, Android device automation, and workflows tailored to this machine or this user's habits.
- Prefer `.agents` for generic methods such as debugging structure, frontend design process, test strategy, browser testing, or brainstorming, when no `.codex` skill is clearly more specific.
- If still unsure, choose `.codex` first, then add the relevant `.agents` skill only if it provides clear execution value.

Explicit examples:

- Requests to commit, push, publish a branch, or prepare git delivery should trigger the `commit-push` skill before running raw git commands.
- Requests to review code or a diff should trigger the code-review-oriented workflow/skill before giving a summary.
- Requests to debug a failing app, test, or build should trigger the debugging skill before ad hoc trial-and-error.
- Requests to control an Android device or app should trigger the Android skill path first, not direct unspecialized probing.

## Android Skill Routing

- Treat any request to control, inspect, or automate the user's Android device as an explicit trigger for the `android-remote` skill.
- Treat any request involving the Android YouTube app as an explicit trigger for the `android-youtube-automation` after `android-remote`.
- For Android YouTube work, do not skip the skill path by jumping straight to ad hoc ADB probing or screenshot-led fallback.
- Prefer `Appium` or `UIAutomator2` as the default action layer once `adb` connectivity is verified.
- Use `scrcpy` only when live visual confirmation is needed, not as a replacement for selector-backed interaction.
- Use screenshots only as a fallback after the structured inspection path is unavailable or unstable.
- If the request clearly names Android remote control plus a specific Android app, start with the app-specific skill if one exists, while still using `android-remote` as the foundation workflow.
- If the user explicitly asks to "remote" the Android device, assume they want live remote-control workflow first, not a screenshot-first workflow.
- Do not claim an Android action succeeded unless the post-action state was verified from live UI, UI hierarchy, foreground activity, or another app-specific success signal.
- If the state is ambiguous, report the ambiguity directly instead of inferring success from a command returning `ok`.
- For Android app automation, treat "command delivered to top-most instance" as non-verifying; it does not prove the intended UI transition happened.
- If the workflow hits repeated ambiguous states, stop calling the task complete and report the exact boundary of what was and was not verified.
- If selector inspection, UI dump, and visual confirmation all fail for the same target, stop with a blocker instead of escalating to repeated guess-based coordinate attempts.
- Do not claim a tooling layer is active or usable just because its process was launched; verify attachment or usable output first.
- Reuse known-good local Android automation setup discovered earlier in the same session instead of re-proving every layer from scratch when the setup is still valid.

## Custom Subagents

### `project_analyst`

Use this subagent when the task is about product or project analysis before implementation.

Call `project_analyst` when you need to:
- analyze a feature or business requirement
- map user flow or system flow from a product perspective
- define scope, assumptions, dependencies, or risks
- write acceptance criteria
- break a feature into backlog items, stories, or tasks
- prepare story sizing in spreadsheet form when the user wants detailed acceptance criteria output similar to an existing sizing sheet

Do not use `project_analyst` for detailed backend API contract design when the main need is request payloads, response schemas, error formats, or endpoint behavior. Use `solutioning_analyst` for that.

Example prompts:
- "Use `project_analyst` to break this feature idea into flow, acceptance criteria, and backlog."
- "Spawn `project_analyst` to analyze the scope and list risks and open questions."
- "Use `project_analyst` to turn this feature into a story sizing sheet like the Example tab."
- "Use `project_analyst` to produce detailed positive flow and negative flow acceptance criteria in sheet form."

Output storage:
- Save `project_analyst` results in spreadsheet form for session-based tracking.
- Default template: `/home/fatihmuhamadridho/.codex/artifacts/templates/project-analysis-template.csv`
- Preferred usage: duplicate the template into a dated working file and update one row per analyzed feature or item.
- When the task is specifically story sizing, prefer writing directly to the target Google Sheet tab using an acceptance-criteria layout with `No`, `Acceptance Criteria`, `Given`, `When`, and `Then`, and make sure the sheet covers detailed positive flow and negative flow rows instead of a compressed summary.

### `solutioning_analyst`

Use this subagent when the task is about backend solution analysis or API design before implementation.

Call `solutioning_analyst` when you need to:
- analyze backend requirements for a feature
- define API flow end-to-end
- design request payloads, headers, params, and validation rules
- define response schema, status codes, and error contract
- analyze downstream integrations, idempotency, retries, side effects, or state changes
- break backend work into technical backlog items

Do not use `solutioning_analyst` for general product discovery, user journey framing, or acceptance criteria that are primarily product-facing unless backend contract design is the main concern. Use `project_analyst` for that.

Example prompts:
- "Use `solutioning_analyst` to define the API contract for this feature, including payload and response format."
- "Spawn `solutioning_analyst` to analyze backend flow, validation, error handling, and integration dependencies."

Output storage:
- Save `solutioning_analyst` results as Markdown technical analysis.
- Default template: `/home/fatihmuhamadridho/.codex/docs/solutioning/templates/solutioning-analysis-template.md`
- Preferred usage: create a dated file per feature under `docs/solutioning/<feature-slug>/`.

@C:\Users\fatih\.codex\RTK.md
