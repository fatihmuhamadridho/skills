# RTK - Rust Token Killer (Codex CLI)

**Usage**: Token-optimized CLI proxy for shell commands.

## Rule

Always prefix shell commands with `rtk`.
Prefer `rtk` for normal commands and reserve `rtk proxy` for compatibility/debugging cases where raw execution is genuinely required.
Avoid `rtk proxy` for routine inspection, status, or filesystem commands unless a specific failure requires raw execution.
Prefer RTK-native subcommands first: `rtk grep`, `rtk read`, `rtk find`, `rtk tree`, `rtk git`, `rtk pnpm`, and other built-in wrappers. Use `rtk ls` only if a native `ls` binary is available on PATH.
For file reads, keep plain reads context-preserving by default: use `rtk read -l minimal` for ordinary reads, `rtk read -l aggressive --max-lines <N>` only when the task is explicitly partial, and `--tail-lines <N>` for end-of-file checks.
Use these common rewrites by default: `Get-Content`/`cat` -> `rtk read -l minimal`, `Get-Content ... -TotalCount N` -> `rtk read -l aggressive --max-lines N`, `Get-Content ... -Tail N` -> `rtk read -l aggressive --tail-lines N`, `rg --files [path]` -> `rtk find [path]`, and bare `Get-ChildItem -Force [path]` -> `rtk find [path]`.
Route the remaining frequent families through RTK too: `rg <pattern>` -> `rtk grep`, `Select-String -Path ... -Pattern ...` or `Select-String -Pattern ... -Path ...` -> `rtk grep`, `Select-String ... -Context A,B` -> `rtk grep <pattern> <path> -B A -A B`, `Get-ChildItem <path> | Where-Object { $_.Name -like 'foo*' }` -> `rtk find <path> -name 'foo*'`, `adb ...` -> `rtk summary adb ...`, `Get-CimInstance`/`Get-PSDrive`/`Get-Process` -> `rtk summary C:\tools\rtk\psinspect.cmd <topic>`, `git status|diff|commit|push` -> `rtk git ...`, and `pnpm ...` -> `rtk pnpm ...`.
Do not emit `rtk proxy powershell -NoProfile -Command "Get-Content ..."` or `rtk proxy powershell -NoProfile -Command "Select-String ..."` for routine code/file inspection. Rewrite them to RTK-native `read` or `grep` forms first.
Do not force `rtk` onto unsupported shell launchers or built-ins such as `powershell`, `pwsh`, `cmd`, `where`, `echo`, `type`, or `Get-Command`; if RTK has no native wrapper or rewrite path, let the shell handle the command natively instead of creating fallback noise.

Examples:

```bash
rtk git status
rtk cargo test
rtk npm run build
rtk pytest -q
```

## Meta Commands

```bash
rtk gain            # Token savings analytics
rtk gain --history  # Recent command savings history
rtk proxy <cmd>     # Run raw command without filtering; last resort only
rtk grep TODO src   # Native search path with RTK filtering
rtk read -l minimal README.md  # Native file read path with context-preserving filtering
```

## Verification

```bash
rtk --version
rtk gain
which rtk
```
