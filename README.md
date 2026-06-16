# Local AI Agent Safety Kit

> [!NOTE]
> **Non-Official Educational Checklist**
> This repository is a community-driven, defensive educational resource. It is not affiliated with, authorized, or endorsed by OpenAI, Anthropic, Google, GitHub, Cursor, Docker, or any other vendor. This kit does not provide security guarantees.

---

## 🎯 Target Audience & Scope
This kit provides practical security checklists, isolation guidelines, and credential-guarding workflows for:
* **Individual developers** integrating terminal-based AI agents into their local workflows.
* **Small engineering teams** auditing security implications of agent adoption in their workspaces.

---

## 🛠️ Supported Contexts & Tools
While not claiming complete coverage, the concepts in this kit apply broadly to local AI coding agents and execution environments, including:
* CLI Agents: Codex, Claude Code, Gemini CLI, Antigravity.
* Editor Extensions: Cursor, Cline, Roo Code.
* Extensible protocols: MCP (Model Context Protocol) servers and custom tool harnesses.

---

## 🚀 Quick-Start Checklist (Top 10 Actions)

Before launching or authorizing an AI agent inside a local repository, run through these ten rapid checks:

1. [ ] **Isolation**: Run the agent in an isolated directory structure (or Docker/VM sandbox) rather than your root user directory.
2. [ ] **Branch Protection**: Initialize a clean git state and checkout a separate scratch/sandbox branch before launching the agent.
3. [ ] **Secret Hiding**: Add all `.env` files, `.git/config`, SSH keys, and cloud credentials to the project `.gitignore`.
4. [ ] **Prompt Approvals**: Turn **on** write-approval prompts in the agent configuration (do not run with blanket bypass permissions by default).
5. [ ] **Keychains**: Keep active API tokens out of directories the agent can read.
6. [ ] **History Cleanup**: Check that shell history configuration does not write keys in plain-text to files the agent can inspect.
7. [ ] **Log Verification**: Truncate or monitor agent session logs to verify no credentials are being output in plain text.
8. [ ] **MCP Allowlist**: Only connect to trusted MCP servers running on your local machine; verify port boundaries.
9. [ ] **Pre-commit Hooks**: Run pre-commit secret scanners to detect keys before pushing any code written by agents.
10. [ ] **Commit Small**: Commit files in small, logical chunks so you can easily review diffs and roll back accidental filesystem writes.

---

## 📂 Kit Documents
* **[Agent Permission Checklist](agent_permission_checklist.md)**: Questions to answer before running an agent in a project.
* **[Secrets & Env Guardrails](secrets_and_env_guardrails.md)**: Defensive rules to isolate credentials, keychains, and tokens.
* **[Sandbox Modes Comparison](sandbox_modes_comparison.md)**: Conceptual overview of filesystem containment modes.
* **[Audit & Rollback Workflow](audit_log_and_rollback_workflow.md)**: Git-based workflows to trace changes and undo mistakes.
* **[Workspace Isolation Template](workspace_isolation_template.md)**: Safe directory structures and docker configs.
* **[MCP Server Risk Checklist](mcp_server_risk_checklist.md)**: How to verify local tool integration safety.
* **[Disclaimer](DISCLAIMER.md)**: Responsibility statements and liability disclaimers.
