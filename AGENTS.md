# Codex Global Workflow Rules

These rules apply to Codex and any agent that reads `.codex/AGENTS.md`.

## Shell and RTK

- Prefix supported external shell commands with `rtk`. In command chains, prefix each supported command separately; do not prefix shell operators or built-ins.
- Prefer RTK-native commands: `rtk read` for files, `rtk find`/`rtk tree` for filesystem inspection, and `rtk git` for Git operations. Use `rtk ls` only if native `ls` is available.
- For searches, use `rtk grep` when its underlying executable is available; if it fails because `grep` is unavailable, use `rtk rg`. Use `rtk find` for `rg --files` and filesystem searches.
- For ordinary reads, use `rtk read -l minimal`. Use `-l aggressive --max-lines <N>` only for explicitly partial reads and `--tail-lines <N>` for end-of-file checks.
- Common rewrites: `Get-Content`/`cat` → `rtk read`; `Select-String`/`rg <pattern>` → `rtk grep` (fallback `rtk rg`); `Get-ChildItem` searches → `rtk find`; `adb` → `rtk summary adb`; `Get-CimInstance`/`Get-PSDrive`/`Get-Process` → `rtk summary <resolved-psinspect-path> <topic>`; `git status|diff|commit|push` → `rtk git`; `pnpm` → `rtk pnpm`.
- Use `rtk proxy <cmd>` only as a last resort for compatibility or debugging when RTK cannot run the command. Do not use it for routine inspection or status. Do not force RTK onto unsupported launchers or built-ins such as `powershell`, `pwsh`, `cmd`, `where`, `echo`, `type`, or `Get-Command`; use the native shell when no RTK path exists.
- Prefer POSIX shell syntax in Git Bash/MSYS on Windows. Resolve `rtk` from `PATH` or environment configuration; never assume its installation path.
- Read `RTK.md` beside this file and follow it.

## Portable Paths

- Resolve `<codex-home>` from `CODEX_HOME`; if unset, use `.codex` under the current user's home directory. Resolve home through the active environment (`HOME`, `USERPROFILE`, or the platform API), never a hardcoded username or drive.
- Global `.codex/skills` means `<codex-home>/skills`; global `.agents/skills` means `<user-home>/.agents/skills`. Resolve project-local skill directories from the project root and use exact session-catalog paths when available.
- Resolve relative document references from their containing document. Resolve project outputs from the project root. Expand placeholders, quote paths, and use syntax for the active shell.
- Resolve helper tools from `PATH` or environment configuration. If unavailable, use a supported platform equivalent; do not assume machine-specific paths.

## Tooling Preferences

- Prefer `pnpm` over `npm` for Node.js package management and CLI installs; use `npm` only when no workable `pnpm` path exists.
- Install Python packages with `python -m pip`, not bare `pip`.

## Memory

- Before non-trivial work, read `memories/memory_summary.md` to identify relevant project context, user preferences, and memory pointers.
- Identify the active project from the current working directory and task. Search `memories/MEMORY.md` for the matching project or topic; avoid pulling in unrelated project sections.
- Follow pointers to rollout summaries or source notes only when their details are needed to answer the task or guide implementation.
- Do not routinely read all of `memories/raw_memories.md` or the full `memories/rollout_summaries` archive.
- Treat memory as historical context, not proof of current state. Verify changeable facts against the active repository, configuration, or authoritative source before relying on them.
- Do not add, edit, or delete memory files unless the user explicitly asks for a memory update.

## Skill Routing

- At the start of every task, check the session skill catalog for a directly relevant skill. Also inspect skill names and descriptions in both the resolved global `<codex-home>/skills` and `<user-home>/.agents/skills` directories, plus project-local `.codex/skills` and `.agents/skills` when present. Use portable path resolution from the Portable Paths rules; do not assume fixed usernames or drive letters.
- Always use at least one directly relevant skill when one is available, even for a small task. Prefer the best task fit; use `.codex` as the starting preference when candidates are equally specific, but never skip a relevant `.agents` skill because a `.codex` skill exists. Combine skills from both sources when each contributes a distinct, needed workflow. Do not use irrelevant skills just to satisfy a source quota or read every skill body.
- Before substantive work, map the task's phases and concerns to relevant skills (for example: requirements, repository conventions, architecture, implementation, accessibility, performance, debugging, verification, and delivery). Select all skills that contribute a distinct required workflow; do not stop after the first match.
- There is no maximum skill count per prompt or task. Use 4–8 skills or more when the scope warrants them. One skill is sufficient only when it covers all applicable needs; do not treat one primary skill as a substitute for complementary skills. Select by coverage, not a target count, and avoid duplicate workflows from equivalent skills.
- Briefly state the selected skills and their responsibilities before substantive work. For multi-phase tasks, add relevant skills as each phase begins; do not limit selection to the initial phase or the first skill's recommendations.
- Read each selected skill's `SKILL.md` before the work it governs, follow its workflow and required references/tools/checks, and briefly tell the user which skill is being applied when doing so helps orient the work. Recheck skill routing when the task moves into a phase with different needs, such as implementation, debugging, testing, review, or delivery.
- If no available skill fits, or a needed skill is unavailable or blocked, state that limitation briefly and use the best authorized fallback. Do not claim to have followed a skill that was not read.
- Apply the selected skills to the actual work: reading or naming a skill alone does not satisfy this rule. Reuse a previously read skill only while its instructions remain available in context; reread it when the instructions are missing, compacted away, or changed.
- Before reporting completion, check that every applicable phase and concern was covered, verify the selected skills' required steps and checks, and state any skipped or blocked requirements accurately. If a coverage gap appears, load and apply the missing skill before claiming completion.

@C:\Users\fatih\.codex\RTK.md
