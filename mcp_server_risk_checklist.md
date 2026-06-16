# MCP Server Risk Checklist

The Model Context Protocol (MCP) enables AI agents to connect to local tools, databases, and APIs. While powerful, this protocol introduces privilege boundaries that must be audited.

---

## 📋 MCP Security Checklist

Before connecting your agent to a local or remote MCP server, audit the following risks:

### 1. Tool Permission Overreach
* **The Risk**: An MCP server may register commands allowing full read/write/delete access on your filesystem.
* **Audit Check**:
  * [ ] Review the server's registered schema definition.
  * [ ] Verify that permissions conform to the principle of least privilege (e.g. read-only database queries instead of full write queries).

### 2. Untrusted Third-Party Servers
* **The Risk**: Running pre-built MCP servers from unverified GitHub repos or npm packages can execute arbitrary code on your machine.
* **Audit Check**:
  * [ ] Only run MCP servers from trusted sources or after inspecting the server script source code.
  * [ ] Run third-party servers inside containerized environments where system access is blocked.

### 3. Local Command Execution
* **The Risk**: Servers designed to run shell utilities (e.g. terminal execution servers) give the agent direct control over your system shell.
* **Audit Check**:
  * [ ] Turn **on** explicit prompts for tool/command execution in your agent IDE.
  * [ ] Disable server hooks that authorize automatic command execution without human-in-the-loop review.

### 4. Network Boundaries & Data Exfiltration
* **The Risk**: A compromised server could query local directories and exfiltrate credentials by sending outbound network requests.
* **Audit Check**:
  * [ ] Enforce firewall rules restricting local MCP servers from opening unsolicited outbound connections.
  * [ ] Monitor server execution logs to verify no data is being sent to external IPs.

---

## 💡 Allowlist-First Strategy

When configuring your agent's MCP JSON config files:
1. **Disable by Default**: Keep all unused server blocks disabled or commented out.
2. **Explicit Allowlist**: Only enable specific tools (such as local file reading or database schema inspects) when actively needed, and disable them immediately after.
