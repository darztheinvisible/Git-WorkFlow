# Git-WorkFlow — Complete Repository Setup Guide

**Platform:** Linux Mint  
**Tools:** Git, GitHub CLI (`gh`), VS Code

## Part 1 — One-Time Computer Setup

These commands only need to be run once per computer, unless your account or settings change.

### Step 1: Check Git and GitHub CLI

```bash
git --version
gh --version
```

### Step 2: Authenticate with GitHub

```bash
gh auth login
```

Follow the prompts to sign in using your browser.

To verify your login:

```bash
gh auth status
```

### Step 3: Set your Git identity globally

This uses your GitHub username and GitHub no-reply email to protect your personal email address.

```bash
git config --global user.name "$(gh api user --jq .login)"

git config --global user.email "$(gh api user --jq '"\(.id)+\(.login)@users.noreply.github.com"')"
```

Verify:

```bash
git config --global user.name
git config --global user.email
```

**Remember:** You do not repeat these commands every time you start a project. Local repository settings can override global ones.

---

## Part 2 — Creating a Brand-New Repository

### Method A: Create the project on your PC first

This is the recommended method when you're starting a new project from scratch.

**Step 1 — Open your Projects folder**

```bash
cd ~/Projects
```

**Step 2 — Create a project directory**

```bash
mkdir Git-Practice
cd Git-Practice
```

Replace `Git-Practice` with whatever you want to call your project.

**Step 3 — Initialize Git**

```bash
git init -b main
```

This creates a local Git repository and names its initial branch `main`.

**Step 4 — Create a README**

```bash
printf '# Git-Practice\n\nMy Git learning repository.\n' > README.md
```

You can also open VS Code:

```bash
code .
```

**Step 5 — Check your files**

```bash
git status
```

**Step 6 — Stage your README**

```bash
git add README.md
```

**Step 7 — Create your first commit**

```bash
git commit -m "Initial commit"
```

This saves your first local project snapshot.

**Step 8 — Create the GitHub repository and push**

```bash
gh repo create Git-Practice --private --source=. --remote=origin --push
```

This command:

- Creates the repository on GitHub.
- Makes it private initially.
- Connects your local Git repository to GitHub.
- Uploads your existing commits.

To create a public repository instead, change `--private` to `--public`.

**Step 9 — Verify**

```bash
git remote -v
git status
```

Your local repository should now be connected to GitHub.

You can view it in your browser with:

```bash
gh repo view --web
```

---

## Part 3 — If You Created the Repository on GitHub First

### Method B: Clone your existing GitHub repository

This is the method to use when you have already created the repository on GitHub, especially if you selected **Add a README**.

**Step 1 — Navigate to Projects**

```bash
cd ~/Projects
```

**Step 2 — Clone your GitHub repository**

```bash
gh repo clone Git-Practice
```

Replace `Git-Practice` with your existing GitHub repository's name.

**Step 3 — Enter the project folder**

```bash
cd Git-Practice
```

**Step 4 — Open VS Code**

```bash
code .
```

Your GitHub files should already be available locally.

**Step 5 — Make your changes**

Create or edit files in VS Code and save them.

**Step 6 — Stage, commit, and push**

```bash
git status
git add .
git commit -m "Add project files"
git push
```

That's it! The repository was already connected when you cloned it.

**Important:** Method A and Method B are alternatives. Do not run both for the same project. Method B assumes the local folder does not already exist.

---

## Part 4 — Everyday Git Workflow

Once your repository is set up, this is what you'll use most often.

**Step 1 — Enter your project**

```bash
cd ~/Projects/Git-Practice
```

**Step 2 — Get recent GitHub changes**

```bash
git pull --ff-only
```

Especially useful if you've edited the repository from another computer. Ideally, do this before making new local changes.

**Step 3 — Work on your files**

Edit your code or documentation in VS Code, then save.

**Step 4 — Check changes**

```bash
git status
```

**Step 5 — Stage the changes**

```bash
git add .
```

This stages new, modified, and deleted files under your current folder.

**Step 6 — Review what's staged**

```bash
git diff --cached --stat
```

**Step 7 — Commit**

```bash
git commit -m "Describe what you changed"
```

Examples:

```bash
git commit -m "docs: update Linux recovery guide"
git commit -m "fix: repair command handler"
git commit -m "feat: add new voice command"
```

**Step 8 — Push to GitHub**

```bash
git push
```

**Step 9 — Confirm everything is synchronized**

```bash
git status
```

Expected result:

```text
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

---

## Part 5 — Useful Git Commands

| Command | What it does |
|---|---|
| `git status` | Shows changed and staged files |
| `git add .` | Stages changes |
| `git commit -m "message"` | Creates a local snapshot |
| `git push` | Uploads commits to GitHub |
| `git pull --ff-only` | Downloads changes without creating a merge commit |
| `git log --oneline -5` | Shows your five latest commits |
| `git remote -v` | Shows connected remote repositories |
| `git diff` | Shows unstaged changes |
| `git diff --cached` | Shows staged changes |
| `gh auth status` | Checks GitHub CLI login |
| `gh repo view --web` | Opens the repository in your browser |

---

## Part 6 — Security and OPSEC

Before pushing a repository, review the staged changes.

Never commit:

- Passwords or API tokens.
- Private SSH keys.
- `.env` files containing secrets.
- Personal information or sensitive system logs.
- Large generated files that don't belong in the repository.

Use `.gitignore` to exclude files you don't want Git tracking.

Remember: Even if you delete a secret later, it may remain accessible in your Git history.

Also, your Git author name and email are stored in commit metadata. Verify your GitHub no-reply identity before publishing public projects.

---

## Quick Reference — The Commands to Memorize

### New local project

```bash
cd ~/Projects
mkdir Git-Practice
cd Git-Practice

git init -b main

printf '# Git-Practice\n' > README.md

git add README.md
git commit -m "Initial commit"

gh repo create Git-Practice --private --source=. --remote=origin --push
```

### Existing GitHub repository

```bash
cd ~/Projects

gh repo clone Git-Practice

cd Git-Practice
code .
```

### Everyday push

```bash
git status
git add .
git commit -m "Describe changes"
git push
```

### Remember the workflow

**Create → Initialize → Stage → Commit → Connect → Push**

After the initial setup:

**Edit → Save → Stage → Commit → Push**

---

*Practice these commands on a test repository until they become familiar. Understanding what each command does is more important than memorizing every option.*
