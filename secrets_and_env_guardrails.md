# Secrets & Environment Guardrails

AI agents can read files, review directory trees, and execute scripts. This document details defensive steps to protect your sensitive credentials from accidental exposure or reading.

---

## 🛡️ Credential Isolation Checkpoints

### 1. Environment Files (`.env`, `.env.local`)
* **The Risk**: Agents scanning the directory structure often parse and read env files to find configuration strings, potentially exposing secrets in prompt logs.
  > [!WARNING]
  > **Session Environment Warning**: Git-ignoring `.env` files only prevents them from being committed to version control. It does not protect secrets already loaded into the current shell history, process environment, active terminal session, or agent runtime context. Agents with process inspection or environment variable reading capabilities can still access active credentials.
* **Defense Checklist**:
  * [ ] Explicitly add `.env`, `.env.*`, and `*.env` to the project `.gitignore`.
  * [ ] Maintain a template file (e.g. `.env.example`) containing only dummy strings (e.g., `API_KEY=YOUR_KEY_HERE`).
  * [ ] Verify the agent ignores `.env` files by requesting it to list variables; it should return a "not found" message.

### 2. API Keys & Session Tokens
* **The Risk**: Copying active secrets into code comments or terminal args can leak them to agent logs.
* **Defense Checklist**:
  * [ ] Never paste raw API keys directly into prompts or codebase files.
  * [ ] Inject keys at run-time using environment variables rather than hardcoding them in files.

### 3. Shell History Files (`.bash_history`, `.zsh_history`)
* **The Risk**: Running inline commands containing password/key arguments exposes them in history logs, which agents can inspect.
* **Defense Checklist**:
  * [ ] Avoid passing plain-text keys in command line flags (use interactive password entry or file-reading methods).
  * [ ] Regularly clear or restrict permissions on history files (`chmod 600 ~/.zsh_history`).

### 4. SSH & Cloud Configs (`~/.ssh`, `~/.aws`, `~/.kube`)
* **The Risk**: Agents running with root or full user privileges can traverse files and read private keys or cloud configuration profiles.
* **Defense Checklist**:
  * [ ] Ensure the user account running the agent has zero read access to `~/.ssh/` or `~/.aws/` directories.
  * [ ] Set files to read-only for system owner only (`chmod 400 ~/.ssh/id_rsa`).

### 5. OS Keychain Access
* **The Risk**: Terminal-based agents executing commands on MacOS or Linux can call keychain access commands (e.g., `security find-generic-password`) if running as the active system user.
* **Defense Checklist**:
  * [ ] Do not authorize agents to run arbitrary keychain query commands.
  * [ ] Enforce prompt checks for command execution to catch unexpected keychain access.
