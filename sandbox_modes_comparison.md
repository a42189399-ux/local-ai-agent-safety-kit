# Sandbox Modes & Containment Comparison

This document provides a conceptual comparison of filesystem containment and execution sandboxing modes when running local AI agents.

---

## 📊 Comparison Matrix

| Sandbox Mode | Filesystem Write Privilege | Command Execution Control | Risk Level | Target Use Case |
| :--- | :--- | :--- | :--- | :--- |
| **Read-Only Mode** | Disabled. Read-only access to specific directory. | Disabled. Agent cannot execute shell commands. | **Low** | Code audits, searching documentation, scanning syntax. |
| **Workspace-Write Mode**| Enabled for project subfolder only. | Disabled or heavily restricted to minor linters. | **Low-Medium**| Writing code, formatting files, editing markdown. |
| **Approval-Based Mode** | Enabled (prompts before write). | Enabled (prompts before running any shell command). | **Medium** | Pair programming, test suite execution, config editing. |
| **Full-Access Mode** | Enabled (no prompts). | Enabled (no prompts; skips permission requests). | **High** (Avoid) | Local developer automation. **Not recommended** for general use. |
| **Container / VM Isolation** | Sandboxed inside container virtual mount. | Sandboxed inside container shell environment. | **Low-Medium**| Processing untrusted files, running external packages. |

---

## ⚙️ Implementing Containment Levels

### Read-Only Constraints
* Ensure the target agent configuration has read flags only.
* Run standard shell commands to verify:
  ```bash
  # Verify directory permissions are read-only for guest accounts if sandbox runs under separate UID
  chmod -R 555 ~/projects/read_only_project
  ```

### Container Isolation (Docker Example)
* Mount only the target folder as a volume:
  ```bash
  # Mount project folder to container workspace
  docker run -it -v ~/projects/target_workspace:/workspace node:latest bash
  ```
  This prevents the agent from traversing folders outside the mounted volume.
