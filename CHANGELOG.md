# Changelog

## [5.0.6] - 2026-03-25

### Added

- **TNG agent suite** — four named agents now ship with superpowers, each assigned a specific model tier for cost-optimised workflows:
  - `geordi` (haiku) — code investigator: reads and traces code, produces structured reports, never modifies files
  - `picard` (opus) — architect/planner: takes investigation reports and produces detailed implementation plans, never writes code
  - `worf` (haiku) — implementer: executes plan tasks with TDD, escalates cleanly via NEEDS_CONTEXT/BLOCKED
  - `riker` (sonnet) — verifier and code reviewer: final plan-level verification plus code quality assessment (merged from `code-reviewer`)
- **Pre-implementation investigation step** in `subagent-driven-development` — dispatch geordi before starting tasks in existing codebases to prevent NEEDS_CONTEXT escalations
- **Codebase investigation section** in `writing-plans` — geordi + picard pathway for non-trivial plans in existing codebases

### Changed

- **`subagent-driven-development` model selection** — replaced generic cheap/standard/capable guidance with a named-agent table (geordi/picard/worf/riker); generic guidance retained as fallback for non-Claude-Code platforms
- **Final review** in `subagent-driven-development` — now dispatches `riker` instead of a generic code-reviewer subagent
- **`implementer-prompt.md`** — notes `worf` as the preferred agent; adds investigation support section directing implementers to report NEEDS_CONTEXT rather than reading files themselves

### Removed

- **`code-reviewer` agent** — capabilities merged into `riker`

## [5.0.5] - 2026-03-17

### Fixed

- **Brainstorm server ESM fix**: Renamed `server.js` → `server.cjs` so the brainstorming server starts correctly on Node.js 22+ where the root `package.json` `"type": "module"` caused `require()` to fail. ([PR #784](https://github.com/obra/superpowers/pull/784) by @sarbojitrana, fixes [#774](https://github.com/obra/superpowers/issues/774), [#780](https://github.com/obra/superpowers/issues/780), [#783](https://github.com/obra/superpowers/issues/783))
- **Brainstorm owner-PID on Windows**: Skip `BRAINSTORM_OWNER_PID` lifecycle monitoring on Windows/MSYS2 where the PID namespace is invisible to Node.js. Prevents the server from self-terminating after 60 seconds. The 30-minute idle timeout remains as the safety net. ([#770](https://github.com/obra/superpowers/issues/770), docs from [PR #768](https://github.com/obra/superpowers/pull/768) by @lucasyhzhu-debug)
- **stop-server.sh reliability**: Verify the server process actually died before reporting success. Waits up to 2 seconds for graceful shutdown, escalates to `SIGKILL`, and reports failure if the process survives. ([#723](https://github.com/obra/superpowers/issues/723))

### Changed

- **Execution handoff**: Restore user choice between subagent-driven-development and executing-plans after plan writing. Subagent-driven is recommended but no longer mandatory. (Reverts `5e51c3e`)
