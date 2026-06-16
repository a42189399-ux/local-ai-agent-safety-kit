# Workspace Isolation Template

This guide details a defensive project workspace layout to separate active code, client documents, credentials, and agent-accessible workspace paths.

---

## 📂 Directory Layout Structure

Configure your project directory to isolate data layers:

```text
my_isolated_project/
├── .gitignore                     # Configured to ignore secrets/scratch paths
├── LICENSE                        # Open-source license file
├── README.md                      # Main developer guide
├── src/                           # Core source code (Read-only for agents during audits)
│   └── app.py
├── data/                          # Target datasets
│   ├── client_uploads/            # Private/sensitive client inputs (IGNORED BY GIT)
│   └── clean_exports/             # Standardized outputs
└── scratch/                       # Dedicated Agent Scratchpad (Read/Write)
    ├── debug_output.json
    └── test_script.py
```

---

## 🛠️ Exclusions & Path Isolation Check

Maintain strict boundaries in your `.gitignore` to prevent leakage:

```text
# Private/Secret client data
data/client_uploads/
data/private/
*.dirty.csv
*.clean.csv
*.private.json

# Local scratch files & temporary logs
scratch/
tmp/
*.log

# Git credentials and configuration envs
.env
.env.*
.vscode/
.idea/
.DS_Store
```

Always verify that the agent's path scope is restricted to the specific subdirectories (e.g. `scratch/`) when editing configuration files.
