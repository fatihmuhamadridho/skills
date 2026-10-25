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

## Skill Routing

- At the start of every task, check the session skill catalog for a directly relevant skill. Also inspect skill names and descriptions in both the resolved global `<codex-home>/skills` and `<user-home>/.agents/skills` directories, plus project-local `.codex/skills` and `.agents/skills` when present. Use portable path resolution from the Portable Paths rules; do not assume fixed usernames or drive letters.
- Always use at least one directly relevant skill when one is available, even for a small task. Prefer the best task fit; use `.codex` as the starting preference when candidates are equally specific, but never skip a relevant `.agents` skill because a `.codex` skill exists. Combine skills from both sources when each contributes a distinct, needed workflow. Do not use irrelevant skills just to satisfy a source quota or read every skill body.
- Read each selected skill's `SKILL.md` before the work it governs, follow its workflow and required references/tools/checks, and briefly tell the user which skill is being applied when doing so helps orient the work. Recheck skill routing when the task moves into a phase with different needs, such as implementation, debugging, testing, review, or delivery.
- If no available skill fits, or a needed skill is unavailable or blocked, state that limitation briefly and use the best authorized fallback. Do not claim to have followed a skill that was not read.
- Before reporting completion, verify applicable skill requirements and checks were followed.
