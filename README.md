# Coding Agents Workshop

Welcome to the **MSOE AI Club Coding Agents Workshop**! In this hands-on walkthrough, you will learn how modern autonomous coding agents work, set up reusable **Agent Skills**, and use a **Spec-Driven Development** workflow to build, verify, and red-team a full-stack Python web application.

---

## Part 1: What Are Coding Agents?

Coding agents represent a shift from **inline text completion** to **autonomous goal execution**:

* **Autocomplete (Copilot / Ghost text):** Predicts the next few syntax tokens inline.
* **Chat Assistants:** Answers questions or writes isolated snippets in a side panel, leaving manual copy/pasting and testing to you.
* **Autonomous Coding Agents:** Given a high-level goal in natural language, they reason through the problem, inspect the repository, write and edit code across multiple files, run terminal commands and tests, and **self-correct when errors occur** using a continuous **ReAct (Reason + Act + Observe)** feedback loop.

---

## Step 1: Install Antigravity (or Bring Your Own Coding Agent)

You can complete this workshop using **Google Antigravity** (which offers free Pro access for students) or **any coding agent you already have installed** (such as Claude Code, Cursor, or GitHub Copilot).

### Option A: Google Antigravity (Recommended)

1. **Student Plan Verification (Free Pro Tier):**
   * Go to [gemini.google/students](https://https://gemini.google/students/) and click on **Claim Free Student Plan**.
   * Follow the steps to claim your free offer using your `.edu` student account to activate the 1-year free Pro tier credit allowance. (*Note:* even if you previously signed up for the free year for Google AI Pro, you may be able to redeem this again as a student)
2. **Download & Install:**
   * Download the installer for your OS (Windows, macOS, or Linux) from [antigravity.google/download](https://antigravity.google/download).
   * Launch Antigravity and sign in with your verified account.

### Option B: Bring Your Own Agent

* If you already use **Claude Code**, **Cursor**, **Windsurf**, or **VS Code with GitHub Copilot Agent Mode**, open your preferred tool and make sure you have **Python 3.10+** and **Git** available in your terminal.

<span style="color:#ff5555;font-weight:bold;font-size:1.5rem;">
    🛑 STOP
</span>

... or keep going if you want to work ahead.

---

> **Checkpoint:** Make sure your coding agent is installed, signed in, and ready to open a project folder.  
> *Done early? Check that `python --version` and `git --version` work in your terminal.*

---

<span style="color:#55ff55;font-weight:bold;font-size:1.5rem;">
    🟢 GO
</span>

---

## Step 2: Clone the Workshop Repository

Clone this repository to your machine and open the folder in Antigravity (or your chosen IDE/agent):

```bash
git clone https://github.com/MSOE-AI-Club/coding-agent-workshop
cd coding-agent-workshop
```

Take a quick look around the repository:
* `skills/` — Contains the prebuilt agent skills we will use during the workshop.
* [`skill-template.md`](skill-template.md) — A starter template for creating your own custom agent skills.
* `README.md` — This workshop walkthrough!

<span style="color:#ff5555;font-weight:bold;font-size:1.5rem;">
    🛑 STOP
</span>

... or keep going if you want to work ahead.

---

> **Checkpoint:** Make sure you have the `coding-agent-workshop` folder open as your active workspace in your IDE/agent.  
> *Done early? Browse the folders inside `skills/` to see the 5 skills we'll be using.*

---

<span style="color:#55ff55;font-weight:bold;font-size:1.5rem;">
    🟢 GO
</span>

---

## Step 3: Set Up Your Agent's Skills Folder

### What is a Skill?

A **skill** is a folder containing a `SKILL.md` file. Each `SKILL.md` file has:
1. **YAML Frontmatter** at the top (`name` and `description`). The agent always sees this short description.
2. **Step-by-Step Instructions** in the body. When your prompt matches the skill's description (or you ask for the skill by name), the agent reads the full `SKILL.md` file and follows its steps.

### The Workshop Workflow

Rather than giving an agent a vague one-sentence prompt and hoping for the best, we use **Spec-Driven Development**. These skills pass work to each other through files in your project:

| Step | Skill | Reads | Writes |
| --- | --- | --- | --- |
| 1. Requirements | [`write-spec`](skills/write-spec/SKILL.md) | your request | `SPEC.md` |
| 2. Plan | Plan Mode (built in) | `SPEC.md` | a plan |
| 3. Check the plan | [`plan-review`](skills/plan-review/SKILL.md) | `SPEC.md`, the plan | a coverage table |
| 4. Build | the agent, no skill | `SPEC.md`, the plan | code and tests |
| 5. Check the work | [`verify`](skills/verify/SKILL.md) | `SPEC.md`, the code | evidence for each criterion |
| 6. Try to break it | [`adversarial-review`](skills/adversarial-review/SKILL.md) | `SPEC.md`, the diff | `REVIEW.md` |
| 7. Fix | [`fix-findings`](skills/fix-findings/SKILL.md) | `REVIEW.md` | fixes and tests |

**Why each step matters:**
* A clear spec gives the agent a concrete target, and gives you something objective to check its work against.
* A bad plan is cheap to fix. Bad code is not.
* Passing tests can still miss a requirement. Always ask for **evidence**.
* The agent that wrote the code tends to trust its own assumptions. We run Step 6 (`adversarial-review`) in a **new chat** so the reviewer tries to break the app with fresh eyes.

### Install the Skills for Your Agent

Copy the skill folders from `skills/` into the folder where your coding agent discovers skills:

* **Antigravity:** `.agent/skills/` in your project (or `~/.gemini/antigravity/skills/` for all projects)
* **Claude Code:** `.claude/skills/` in your project

**macOS / Linux (Bash/Zsh):**
```bash
# For Antigravity:
mkdir -p .agent/skills && cp -r skills/*/ .agent/skills/

# For Claude Code:
mkdir -p .claude/skills && cp -r skills/*/ .claude/skills/
```

**Windows (PowerShell):**
```powershell
# For Antigravity:
New-Item -ItemType Directory -Force -Path .agent/skills
Get-ChildItem -Path skills -Directory | Copy-Item -Destination .agent/skills -Recurse -Force

# For Claude Code:
New-Item -ItemType Directory -Force -Path .claude/skills
Get-ChildItem -Path skills -Directory | Copy-Item -Destination .claude/skills -Recurse -Force
```

*(Tip: Even simpler—you can also ask your coding agent in chat: `"Copy all the skill folders inside skills/ into .agent/skills/"`!)*

> 💡 **Using another agent?** If your agent doesn't use `.agent/skills/` or `.claude/skills/`, you can still use every skill in this workshop by tagging or mentioning the file directly in your prompt (e.g., `"Follow the instructions in @skills/write-spec/SKILL.md"`).

### Quick Check: Read Your First Skill

Before moving on, open [`skills/write-spec/SKILL.md`](skills/write-spec/SKILL.md) and read through it. Notice how short it is—would a new teammate understand these steps?

<span style="color:#ff5555;font-weight:bold;font-size:1.5rem;">
    🛑 STOP
</span>

... or keep going if you want to work ahead.

---

> **Checkpoint:** Confirm that `.agent/skills/` (or `.claude/skills/`) contains all 5 skill folders: `write-spec`, `plan-review`, `verify`, `adversarial-review`, and `fix-findings`.  
> *Done early? Read through `skills/write-spec/SKILL.md` and `skills/plan-review/SKILL.md`.*

---

<span style="color:#55ff55;font-weight:bold;font-size:1.5rem;">
    🟢 GO
</span>

---

## Step 4: Start Your Full-Stack Python App with a Spec (`write-spec`)

Now we're ready to build! We'll create a lightweight **full-stack Python web app** using **Flask or FastAPI + SQLite + a single-page HTML/JS frontend**. Having only Python dependencies (`flask` or `fastapi`/`uvicorn` + `pytest`) keeps setup fast while letting the agent test both backend API routes and a live browser UI.

1. Read [`skills/write-spec/SKILL.md`](skills/write-spec/SKILL.md) so you know what to expect.
2. Give your agent a prompt to write a `SPEC.md` for your app. You can copy the starter prompt below, or swap in your own app idea (e.g., a study group finder, a campus event RSVP board, a bug tracker, or a flashcard quiz app):

```text
Use the write-spec skill to write a SPEC.md for a lightweight full-stack Python web app: a Campus Event Board & RSVP Tracker.

Tech stack constraints:
- Backend: Python with FastAPI (or Flask) and a local SQLite database
- Frontend: A single-page HTML/CSS/JS interface served directly by the Python backend
- Testing: pytest for backend API and edge-case tests

Core features:
1. Users can create an event with a title, date, capacity limit, and allowed email domain (e.g., only @msoe.edu emails).
2. Users can view all upcoming events and how many spots remain.
3. Users can RSVP to an event with their name and email address, which validates their email domain, prevents duplicate RSVPs for the same event, and blocks RSVPs once the event hits capacity.
```

3. **Answer the agent's clarifying questions:** The `write-spec` skill tells the agent to ask at most 3 questions that affect what gets built, and record the rest as assumptions.
4. **Review `SPEC.md`:** Make sure every Requirement (`R1`, `R2`...) has clear, testable Acceptance Criteria (`AC1`, `AC2`...) and at least 3 Edge Cases (`E1`, `E2`, `E3`...). Do **not** let the agent write application code yet!

<span style="color:#ff5555;font-weight:bold;font-size:1.5rem;">
    🛑 STOP
</span>

... or keep going if you want to work ahead.

---

> **Checkpoint:** Make sure `SPEC.md` is saved in your project root and you've approved its requirements and edge cases.  
> *Done early? Check the `## Edge cases` section of your `SPEC.md`—try adding one sneaky edge case of your own before moving to Step 5!*

---

<span style="color:#55ff55;font-weight:bold;font-size:1.5rem;">
    🟢 GO
</span>

---

## Step 5: Plan & Review the Plan (`Plan Mode` + `plan-review`)

A bad plan is cheap to fix; bad code is not. Before building the app, let's have the agent propose a plan and verify that it covers every requirement in `SPEC.md`.

1. **Create the Plan:**
   * Switch your agent to **Plan Mode** (or ask it: *"Read `SPEC.md` and propose a step-by-step implementation plan for building and testing this app. Do not write the code yet."*).
2. **Check the Plan with `plan-review`:**
   * Read [`skills/plan-review/SKILL.md`](skills/plan-review/SKILL.md).
   * Prompt your agent:
     ```text
     Use the plan-review skill to check the implementation plan against SPEC.md.
     ```
3. **Inspect the Coverage Table:**
   * Look at the agent's report. Did the plan cover every Acceptance Criterion (`AC1`...) and Edge Case (`E1`...)?
   * If anything is marked `MISSING` or `REVISE`, tell the agent to update the plan until the verdict is `APPROVE`.

<span style="color:#ff5555;font-weight:bold;font-size:1.5rem;">
    🛑 STOP
</span>

... or keep going if you want to work ahead.

---

> **Checkpoint:** Make sure your `plan-review` coverage table has `Covered` for every row and a verdict of `APPROVE`.  
> *Done early? Read `skills/verify/SKILL.md` to see how the agent will prove its code works after building.*

---

<span style="color:#55ff55;font-weight:bold;font-size:1.5rem;">
    🟢 GO
</span>

---

## Step 6: Build the App & Verify with Evidence (`Build` + `verify`)

Now that you have an approved `SPEC.md` and implementation plan, let the agent build the application!

### 1. Build the Implementation
Prompt your agent:
```text
Implement the approved plan from SPEC.md. Set up a virtual environment (or requirements.txt), build the SQLite database layer, Python backend, and HTML/JS frontend, and write the pytest suite.
```

Watch the agent's **ReAct loop** in action:
* Creating files (`app.py`, `index.html`, `test_app.py`, etc.)
* Running `pytest` in the terminal
* Self-correcting if any initial test or import fails

### 2. Verify Against the Spec (`verify`)
"The tests pass" is **not** the same as "the feature is correct." Passing tests can still miss a requirement or check the wrong thing.

1. Read [`skills/verify/SKILL.md`](skills/verify/SKILL.md).
2. Prompt your agent:
   ```text
   Use the verify skill to check the implementation against SPEC.md and show evidence for every criterion and edge case.
   ```
3. **Review the Evidence:**
   * Check the output table: does every `AC` and `E` item have real proof (a specific test name, terminal command output, or a browser screenshot)?
   * Start the server yourself (or ask Antigravity to open it in the browser) and click around your new web app!

<span style="color:#ff5555;font-weight:bold;font-size:1.5rem;">
    🛑 STOP
</span>

... or keep going if you want to work ahead.

---

> **Checkpoint:** You should have a working full-stack Python app, passing `pytest` tests, and a completed `verify` evidence table.  
> *Done early? Open your app in the browser and try to break it manually by entering weird inputs!*

---

<span style="color:#55ff55;font-weight:bold;font-size:1.5rem;">
    🟢 GO
</span>

---

## Step 7: Try to Break It & Fix Findings (`adversarial-review` + `fix-findings`)

Even after verification passes, subtle bugs often hide in edge cases (like uppercase vs. lowercase email addresses, whitespace-only inputs, negative capacities, or double-clicking submit).

Why didn't the first agent catch them? Because **an agent that wrote the code shares the same blind spots and assumptions in its tests**.

### 1. Run an Adversarial Review in a New Chat
1. Read [`skills/adversarial-review/SKILL.md`](skills/adversarial-review/SKILL.md). Notice Rule #1: *If you wrote this code in this chat, tell the user to start a new chat and run this skill there.*
2. **Open a brand-new chat conversation** in your agent.
3. In the **new chat**, prompt the agent:
   ```text
   Use the adversarial-review skill to try to break this app against SPEC.md and write reproducible findings to REVIEW.md.
   ```
4. Watch the reviewer inspect `SPEC.md` and the code, invent attacks across 5 angles (edge inputs, state, missed requirements, old behavior, weak tests), test them, and write confirmed failures to `REVIEW.md`.

### 2. Fix Confirmed Findings (`fix-findings`)
1. Read [`skills/fix-findings/SKILL.md`](skills/fix-findings/SKILL.md).
2. If `REVIEW.md` has confirmed findings, prompt the agent:
   ```text
   Use the fix-findings skill to reproduce and fix the issues in REVIEW.md, then run verify again.
   ```
3. Notice how `fix-findings` forces disciplined engineering:
   * Reproduce the bug first
   * Write a failing test that proves the bug exists
   * Write the smallest code change to fix it
   * Re-run all tests and `verify`

<span style="color:#ff5555;font-weight:bold;font-size:1.5rem;">
    🛑 STOP
</span>

... or keep going if you want to work ahead.

---

> **Checkpoint:** Check out `REVIEW.md`—what kinds of bugs did your adversarial reviewer uncover that `verify` missed?  
> *Done early? Think about a repetitive task you do in your own projects that could be turned into a custom skill in Step 8.*

---

<span style="color:#55ff55;font-weight:bold;font-size:1.5rem;">
    🟢 GO
</span>

---

## Step 8: Level Up — Improve a Skill & Build Your Own

You've now run the full Spec-Driven Development loop! To wrap up the hands-on exercises, customize your agent's toolkit:

### Challenge 1: Improve One Skill Instruction
1. Pick a step from earlier where the agent did something vague, skipped a detail, or needed extra nudging.
2. Save a copy of the agent's original output.
3. Edit the corresponding `SKILL.md` file in `.agent/skills/` (or `.claude/skills/`) to make the instruction sharper.
4. Run the skill again on the same input and compare the before-and-after reports!

### Challenge 2: Make a New Skill of Your Choice
Create a new folder and file at `.agent/skills/<your-skill-name>/SKILL.md` (or `.claude/skills/<your-skill-name>/SKILL.md`). You can copy the provided [`skill-template.md`](skill-template.md) as a starting point.

Remember:
* The `name` in the YAML frontmatter must match the folder name and use only lowercase letters, numbers, and hyphens.
* The `description` should state **what** the skill does and **when** to use it. Keep it specific so it only triggers when intended.
* The steps should be short, ordered, handle missing inputs, and ask for **evidence** when done.

**Template ([`skill-template.md`](skill-template.md)):**
```markdown
---
name: your-skill-name
description: Do <specific task> when the user says "<trigger phrase>".
---

# <Skill Title>

## Steps

1. Check for required inputs...
2. Run...
3. Produce report with evidence...

## Rules

- ...
```

**Ideas for your custom skill:**
* `write-readme`: Write setup steps for your new Python web app and test them in a clean environment.
* `commit-message`: Inspect `git diff` and write a conventional commit message in your team's style.
* `explain-code`: Explain a file to a beginner with an ASCII diagram and one small example.
* `ui-check`: Open the web app at mobile and desktop widths and take screenshots of each state.
* `dependency-check`: Audit `requirements.txt` before adding a library (license, size, maintenance status).

---

## Bonus: Tips, Tricks & Advanced Workflows

### A. Model Context Protocol (MCP) Servers
* **What is MCP?** An open standard that connects coding agents to external tools, databases, APIs, and documentation without custom glue code.
* **Why use MCP?**
  * **Database Inspection:** Connect an MCP server to PostgreSQL/SQLite so the agent inspects live schemas instead of guessing column names.
  * **Live Documentation:** Connect documentation search MCPs so the agent references up-to-date library APIs instead of stale training data.
  * **Issue Trackers:** Pull context directly from GitHub Issues, Jira, or Linear.

### B. Workspace Rules (`.agent/rules`)
* While **Skills** are on-demand workflows for specific tasks, **Rules** define always-on project conventions: preferred frameworks (`pytest` over `unittest`), code style, architectural boundaries, and security guardrails.

### C. Prompt Steering Best Practices
1. **Provide Clear Acceptance Criteria:** Avoid vague instructions like *"make this better."* Give measurable targets or use `write-spec` first.
2. **Scaffold Before Delegating:** Give the agent an existing pattern to follow so code style stays consistent.
3. **Never Blindly Accept Diffs:** Always review changes in the git diff view and demand verification evidence.
4. **Use Atomic Commits:** Commit after each verified milestone (`SPEC.md` $\rightarrow$ Initial Build $\rightarrow$ Review Fixes) so rollbacks are effortless.
