# Git Workflow Automation

This project automates a typical Git workflow using a combination of logic and user inputs. It is intended to streamline the process of setting up and maintaining a Git repository for your project.

---

## What It Does

- Checks if Git is initialized in the current directory.
- Accepts:
  - Repository URL
  - Username/password or authentication key
- If not initialized:
  - Scans through the directory
  - Creates a `.gitignore` and `README.md`
  - Adds all files not in `.gitignore`
  - Makes an initial commit with a user-defined theme message
- If already initialized and remote linked:
  - Checks for changes in the root directory
  - Updates `README.md` with details of recent changes
  - Stages and commits the changes with a summary message
- Before pushing:
  - Lists all branches from the remote repository
  - Lets user choose an existing branch or enter a new branch name
  - Pushes changes to that branch (if up to date)

---

## How It Works

The tool follows a decision-based flow to determine the state of the current directory and guide the Git operations. It prompts at appropriate steps and performs the operations automatically wherever possible.

---

## Initial Architecture diagram

```mermaid
flowchart TD
    A[Start Workflow] --> B{Is Git Initialized?}
    
    B -- No --> C[Initialize Git Repo]
    C --> D[Create .gitignore and README.md]
    D --> E[git add .]
    E --> F[git commit -m Initial commit]
    F --> G[Link Remote Repo git remote add origin]

    B -- Yes --> H[Check for File Changes]
    H --> I{Any Changes?}
    I -- Yes --> J[Update README.md with Changes]
    J --> K[git add .]
    K --> L[LLM generates summary commit message]
    L --> M[git commit -m LLM Summary]

    I -- No --> Z[Exit: No changes to commit]

    M --> N[List Branches in Remote]
    N --> O[User Selects/Enters Branch Name]
    O --> P{Does Branch Exist?}
    P -- No --> Q[Create New Branch Locally and Push]
    P -- Yes --> R{Is Local Branch Up To Date?}
    R -- No --> S[Pull or Warn User to Sync]
    R -- Yes --> T[Push to Selected Branch]

    G --> N
    Q --> T
    S --> Z
    T --> Z[End Workflow]
```

---

## Notes

This project is still under development. Core functionality is being implemented step by step, starting from repository detection and basic commit automation.

