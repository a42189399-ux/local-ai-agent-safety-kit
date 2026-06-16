# Agent Permission Checklist

Answer these core questions to evaluate risks before running a local AI agent in your project workspace.

---

## 📋 Pre-Flight Workspace Questionnaire

### 1. Filesystem Access
* **Q: Does the agent have write access to directories outside the active git repository?**
  * *Action*: Limit the agent's workspace path configuration to a specific subfolder.
* **Q: Are there untracked or configuration files (.git, .vscode, .env) in the workspace that the agent could read?**
  * *Action*: Explicitly exclude these patterns in the agent's ignore configuration (`.gitignore` or `.clineignore`).

### 2. Command Execution
* **Q: Does the agent require root/administrator privileges (`sudo`) to execute commands?**
  * *Action*: **Never** run terminal agents as root. Use a dedicated, low-privilege local user account.
* **Q: Does the agent support auto-execution of commands, or does it prompt for approval?**
  * *Action*: Enable manual approval prompts for all bash/terminal commands.

### 3. Network Access
* **Q: Does the agent run local port bindings or access external API endpoints directly?**
  * *Action*: Inspect outbound connections. Block outgoing ports if the agent only requires local processing.
* **Q: Does the agent communicate with third-party servers to format prompts or validate outputs?**
  * *Action*: Confirm the data processing policy of the service.

### 4. Dependency Isolation
* **Q: Can the agent install packages (e.g. `npm install`, `pip install`) directly to your system path?**
  * *Action*: Run the agent inside a Python virtual environment (`venv`) or standard container node.

---

## 🔒 Configuration Cheat Sheet

Keep these standard safety parameters in mind:

| Risk Area | High-Risk Configuration | Lower-Risk Configuration |
| :--- | :--- | :--- |
| **Execution Mode** | Auto-execute / Danger-skip-permissions | Interactive mode / Prompt-for-approval |
| **Write Directory** | System root `/` or User home `~` | Project root `~/projects/sandbox/` |
| **Package Installs** | Global systems (`sudo apt-get`) | Local package locks (`npm install --no-audit`) |
