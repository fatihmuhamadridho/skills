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

- Before substantive work, look for a suitable skill in the session catalog and relevant global or project-local `.codex/skills` and `.agents/skills` directories. Use the skill catalog first, then inspect local skill names and descriptions when needed.
- Choose a skill based on the task, read its `SKILL.md`, and follow its workflow. Recheck when the task moves into a phase with different needs; combine skills only when they add distinct value.
- Respect applicable skill requirements for specialized workflows. If no suitable skill is available or it is blocked, state the limitation and use the best authorized fallback; do not claim to have followed a skill that was not read.
- Do not force a skill onto small or obvious tasks when it adds no value. Before reporting completion, verify applicable skill requirements and checks were followed.
