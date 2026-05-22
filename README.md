# skill-security-review

A skill for reviewing AI-agent skill/plugin repositories for prompt-injection, malicious prompt behavior, and supply-chain risks.

It treats `SKILL.md`, `AGENTS.md`, `CLAUDE.md`, slash commands, MCP configs, and referenced templates/scripts as **executable control-plane code** rather than documentation, and audits them against the [OWASP Top 10 for LLM Applications](https://owasp.org/www-project-top-10-for-large-language-model-applications/).

## What it covers

- Prompt-injection patterns (instruction-override, hidden instructions, tool-output-as-instruction)
- Trust-boundary confusion between author / user / repo / web / tool-output content
- Sensitive data access (env, credentials, cookies, history, memory)
- Supply-chain risks (unpinned installs, lifecycle hooks, curl-pipe-shell, marketplace confusion)
- Excessive agency (broad tool scopes, weak consent gates, auto-commit/push/deploy)
- Persistence vectors (memory/skill writes, shell startup, git hooks)
- Exfiltration paths (webhooks, telemetry, encoded side channels)
- Secret scanning across the working tree and git history

Each finding is mapped to one or more OWASP LLM Top 10 categories with severity, exploit path, impact, and concrete fix.

## Usage

### As a Claude Code skill

Place this directory under one of:

- `~/.claude/skills/skill-security-review/` (user-global)
- `<repo>/.claude/skills/skill-security-review/` (project-local)

Then ask Claude Code something like:

> Run a security review of the skill repo at `<path-or-url>`.

The skill triggers on requests to audit Claude Code / Hermes / Codex / Gemini skills, `SKILL.md` files, agent plugins, prompt packs, or MCP/tool instructions.

### Standalone

`SKILL.md` is self-contained and can be used as a prompt/checklist by any agent or human reviewer. Open it directly and follow the workflow.

## Workflow (summary)

1. Clone or locate the repo (record commit + remote)
2. Inventory the full attack surface
3. Review prompt files as executable instructions
4. Run focused regex searches for high-risk patterns
5. Scan secrets in tree + history
6. Check install and supply-chain flows
7. Check defensive documentation
8. Red-team each candidate finding for a concrete exploit path
9. Cross-check against OWASP LLM01–LLM10
10. Produce an aggressive hardening pass

See `SKILL.md` for the full procedure, output format, and severity calibration.

## Output

Reviews produce a structured markdown report with:

- Verdict
- Confirmed findings (severity, file:line, confidence, OWASP mapping, exploit path, impact, fix)
- Needs-investigation items
- Prompt-injection assessment
- Supply-chain and install assessment
- Hardening recommendations
- OWASP LLM Top 10 coverage
- Secret scan results
- Attack-surface counts

## Scope and safety

- **Read-only by default.** The skill does static inspection; it does not execute repository code unless the user explicitly asks and the command is low-risk or sandboxed.
- **Adversarial mode by default.** Assumes the audited skill may be loaded by a powerful agent with filesystem, shell, browser, web, memory, and messaging tools.
- Not a substitute for runtime sandboxing, code execution review, or full dependency CVE scanning.

## License

MIT
