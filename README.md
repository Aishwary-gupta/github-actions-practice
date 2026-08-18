# 🚀 GitHub Actions Practice

> Part of my **#90DaysOfDevOps** journey — learning CI/CD and GitHub Actions by building real workflows.

This repository contains my hands-on practice with **GitHub Actions**, starting with my first CI workflow and gradually progressing toward more advanced CI/CD concepts.

The goal is to understand not only how to write GitHub Actions YAML, but also **how workflows execute, how runners work, how failures are diagnosed, and how automation fits into a real DevOps pipeline.**

---

## 📚 Learning Progress

| Day     | Topic                         | Status      |
| ------- | ----------------------------- | ----------- |
| Day 39  | CI/CD Concepts                | ✅ Completed |
| Day 40  | First GitHub Actions Workflow | ✅ Completed |
| Day 41+ | Advanced GitHub Actions       | 🔜 Upcoming |

---

# 🎯 Day 40 — My First GitHub Actions Workflow

Today I created my first GitHub Actions workflow and executed it on a GitHub-hosted Ubuntu runner.

The workflow is triggered automatically whenever code is pushed to the repository.

### Pipeline Flow

```text
Developer
    │
    │ git push
    ▼
GitHub Repository
    │
    │ push trigger
    ▼
GitHub Actions
    │
    ▼
Ubuntu Runner
    │
    ├── Checkout Repository
    │
    ├── Print Hello Message
    │
    ├── Show Date & Time
    │
    ├── Show Branch Name
    │
    ├── List Repository Files
    │
    └── Show Runner OS
    │
    ▼
Workflow Result
    │
    ├── ✅ Success
    └── ❌ Failure
```

---

# 📁 Project Structure

```text
github-actions-practice/
│
├── .github/
│   └── workflows/
│       └── hello.yml
│
└── README.md
```

---

# ⚙️ Workflow

The workflow is located at:

```text
.github/workflows/hello.yml
```

```yaml
name: My First GitHub Actions Workflow

on:
  push:

jobs:
  greet:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Say Hello
        run: echo "Hello from GitHub Actions!"

      - name: Show Current Date and Time
        run: date

      - name: Show Branch Name
        run: |
          echo "Branch: ${{ github.ref_name }}"

      - name: List Repository Files
        run: ls -la

      - name: Show Runner Operating System
        run: |
          echo "Runner OS: $RUNNER_OS"
```

---

# 🧠 Understanding the Workflow

## `name:`

```yaml
name: My First GitHub Actions Workflow
```

Defines the name displayed for the workflow in the GitHub Actions interface.

---

## `on:`

```yaml
on:
  push:
```

Defines the event that triggers the workflow.

In this project:

> Every push to the repository starts a new workflow run.

---

## `jobs:`

```yaml
jobs:
```

Defines the jobs that belong to the workflow.

This workflow contains one job:

```yaml
greet:
```

---

## `runs-on:`

```yaml
runs-on: ubuntu-latest
```

Specifies the operating system used by the GitHub-hosted runner.

In this case, GitHub provides an Ubuntu environment to execute the job.

---

## `steps:`

```yaml
steps:
```

Defines the individual actions and commands executed by the job.

The steps run sequentially unless the workflow is configured differently.

---

## `uses:`

```yaml
uses: actions/checkout@v4
```

Uses an existing GitHub Action instead of writing the functionality ourselves.

`actions/checkout` checks out the repository code onto the runner so later steps can access the project files.

---

## `run:`

```yaml
run: echo "Hello from GitHub Actions!"
```

Executes a shell command on the runner.

For example:

```bash
echo "Hello from GitHub Actions!"
```

---

# 🔍 What This Workflow Demonstrates

[OThe workflow performs the following tasks:

### 1. Checkout Repository

```yaml
uses: actions/checkout@v4
```

Downloads the repository contents onto the GitHub Actions runner.

### 2. Print a Message

```bash
echo "Hello from GitHub Actions!"
```

Confirms that the workflow is executing commands successfully.

### 3. Show Date and Time

```bash
date
```

Displays the current date and time of the runner.

### 4. Show Branch

```yaml
${{ github.ref_name }}
```

Uses GitHub's built-in context to identify the branch that triggered the workflow.

Example:

```text
Branch: main
```

### 5. List Repository Files

```bash
ls -la
```

Displays files and directories available on the runner after checkout.

### 6. Show Runner Operating System

```bash
echo "Runner OS: $RUNNER_OS"
```

Displays the operating system of the GitHub Actions runner.

Example:

```text
Runner OS: Linux
```

---

# ☁️ What Happens Behind the Scenes?

When I run:

```bash
git push
```

the process is approximately:

```text
                    My Computer
                         │
                         │ git push
                         ▼
                 ┌───────────────┐
                 │    GitHub     │
                 │   Repository  │
                 └───────┬───────┘
                         │
                    push detected
                         │
                         ▼
                 ┌───────────────┐
                 │ GitHub Actions│
                 └───────┬───────┘
                         │
                    Create Runner
                         │
                         ▼
                 ┌───────────────┐
                 │ Ubuntu Runner │
                 └───────┬───────┘
                         │
                         ▼
                  Execute Workflow
                         │
                         ▼
                    Result
                  ┌──────┴──────┐
                  │             │
                  ▼             ▼
                 ✅            ❌
               Success        Failed
```

This helped me understand that GitHub Actions is not simply executing commands "inside GitHub."

A runner is the compute environment where the actual commands are executed.

---

# 🧪 Failure Testing

As part of the exercise, I intentionally introduced a failing command:

```yaml
- name: Fail Intentionally
  run: exit 1
```

This caused the workflow to fail.

The Actions interface displayed a failed run with a red ❌ indicator.

The important lesson was:

> A failed pipeline is not necessarily a bad thing. It can mean that CI/CD successfully detected a problem before the application moved further through the delivery process.

After removing the failing command and pushing again, the workflow returned to a successful state.

---

# 🟢 Successful Pipeline

The final workflow completed successfully with a green checkmark.

The successful run verified that:

* GitHub detected the push
* The workflow started correctly
* The Ubuntu runner was provisioned
* The repository was checked out
* Shell commands executed successfully
* GitHub context variables worked
* The workflow completed without errors

### Screenshot

*Add your screenshot of the successful GitHub Actions run here.*

Example:

```text
docs/images/day-40-green-run.png
```

---

# 🧩 Key Concepts Learned

| Concept        | What I Learned                                      |
| -------------- | --------------------------------------------------- |
| Workflow       | Automated process defined using YAML                |
| Trigger        | Event that starts a workflow                        |
| Job            | Unit of work inside a workflow                      |
| Runner         | Machine/environment that executes a job             |
| Step           | Individual action or command                        |
| `uses`         | Reuses an existing GitHub Action                    |
| `run`          | Executes a shell command                            |
| GitHub Context | Provides information about the current workflow/run |
| Workflow Logs  | Used to understand successful and failed steps      |

---

# 🛠️ Technologies Used

* Git
* GitHub
* GitHub Actions
* YAML
* Linux / Ubuntu
* Shell Commands
* GitHub Actions Context Variables

---

# 🎯 Learning Objectives

Through this exercise, I learned how to:

* Create a GitHub repository
* Clone a repository locally
* Create a `.github/workflows/` directory
* Write a GitHub Actions workflow
* Configure a `push` trigger
* Create a job
* Select an Ubuntu runner
* Use `actions/checkout`
* Execute shell commands
* Access GitHub context variables
* Read workflow logs
* Intentionally break a pipeline
* Debug a failed workflow
* Fix the workflow and verify a successful run

---

# 📈 What's Next?

This is only the beginning of my GitHub Actions journey.

Next, I will move toward:

```text
First Workflow
      ↓
Multiple Jobs
      ↓
Dependencies
      ↓
Conditions
      ↓
Environment Variables
      ↓
Secrets
      ↓
Artifacts
      ↓
Testing
      ↓
Docker Builds
      ↓
CI/CD Pipeline
      ↓
Deployment 🚀
```

---

# 🚀 90 Days of DevOps

This repository is part of my **90 Days of DevOps** learning journey, where I am building practical skills in:

```text
Linux
  ↓
Shell Scripting
  ↓
Git & GitHub
  ↓
Docker
  ↓
CI/CD
  ↓
GitHub Actions
  ↓
Kubernetes
  ↓
Cloud & DevOps
```

The goal is not just to learn commands, but to understand **why DevOps tools are used and how they work together in real-world environments.**

---

## 📌 Resources

* [GitHub Actions Documentation](https://docs.github.com/en/actions)
* [GitHub Actions Workflows](https://docs.github.com/en/actions/concepts/workflows-and-actions/workflows)
* [actions/checkout](https://github.com/actions/checkout)

---

## 👨‍💻 Author

**Aishwary Gupta**

BCA Student | Aspiring Cloud & DevOps Engineer

Currently learning and building through the **#90DaysOfDevOps** challenge.

---

## ⭐ Progress

If you're also learning DevOps, feel free to explore the repository and follow along with the journey.

**Learn → Build → Break → Debug → Improve → Repeat.**

---

### Tags

`#90DaysOfDevOps` `#DevOpsKaJosh` `#TrainWithShubham` `#DevOps` `#GitHubActions` `#CICD` `#CloudComputing` `#DevOpsJourney`
# github-actions-practice
