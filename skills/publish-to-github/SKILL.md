---
name: publish-to-github
description: "Use this skill when the user is ready to push their local AI skill or rule repository to GitHub for the first time. This includes running git init and the initial commit, creating the GitHub repository via the gh CLI or manual browser steps, setting the repository description and Topics tags, and confirming the push succeeded. Trigger when the user says 'upload to GitHub', 'push this to GitHub', 'create my repo on GitHub', 'how do I publish this', 'make this live', or 'I have all the files ready, now what'. Also trigger when the user has a complete local folder with all files and just needs the final publishing step. Do NOT trigger if any files still need to be written or edited before publishing — all content must be finalized first. Do NOT trigger for submitting the repo to awesome-lists after publishing (that is submit-to-directories)."
license: MIT
---

## Quick Start

If all files are ready and the user has `gh` installed:

```bash
git init && git add . && git commit -m "feat: initial release of {name}"
gh repo create {username}/{repo} --public --description "{tagline}" --source=. --push
```

Then set Topics on the repo page (Phase 3). Done.

If `gh` is not installed, follow the manual path in Phase 2.

---

## Pre-flight checklist

Confirm all of these before running any git commands:

- [ ] `README.md` exists with no unfilled `{PLACEHOLDER}` tokens
- [ ] `LICENSE` file exists
- [ ] Rule file exists (SKILL.md / .cursorrules / .windsurfrules / AGENTS.md)
- [ ] `assets/banner.svg` and `assets/banner-dark.svg` exist (or README has no banner reference)
- [ ] `.gitignore` exists (see template below)
- [ ] Repo name is decided (kebab-case, no spaces)
- [ ] GitHub username is known

If anything is missing, stop and complete it first.

---

## .gitignore template

```
# OS
.DS_Store
Thumbs.db

# Editors
.vscode/
.idea/
*.swp

# Python (if repo has scripts)
__pycache__/
*.pyc
.venv/
.env

# Node (if repo has tools)
node_modules/
dist/
```

---

## Phase 1: Initialize and commit

```bash
cd {repo-name}
git init
git add .
git status       # review before committing
git commit -m "feat: initial release of {skill-name}

- {rule-file}: {one-line description}
- README: install instructions and usage
- LICENSE: {MIT|CC0}"
```

Commit message rules:
- First line: `feat: initial release of {name}` (50 chars max)
- Body: bullet list of what's included
- NEVER use "Initial commit"

---

## Phase 2: Create the GitHub repository

### With `gh` CLI

```bash
gh repo create {github-username}/{repo-name} \
  --public \
  --description "{one-line description}" \
  --source=. \
  --push
```

### Manual (no `gh` CLI)

1. Go to https://github.com/new
2. Fill in: repo name, description, Public. Do NOT initialize with README or license.
3. Click "Create repository"
4. Run:

```bash
git remote add origin https://github.com/{github-username}/{repo-name}.git
git branch -M main
git push -u origin main
```

---

## Phase 3: Configure GitHub metadata

Do this on the repo page immediately after pushing. Topics directly affect search ranking.

Click the gear icon next to "About" and add:

```
# Platform (pick what matches your rule file format)
cursor, cursorrules, cursor-ai         ← Cursor
windsurf, windsurf-ai, windsurfrules  ← Windsurf
claude, claude-code, claude-skills    ← Claude
agents-md, ai-agents                  ← AGENTS.md
github-copilot                        ← Copilot

# Always include
ai, llm, rules

# For collection repos
awesome, awesome-list

# Domain tags (add as applicable)
security, typescript, python, react...
```

Aim for 6–12 topics. Fewer than 5 hurts search visibility; more than 15 dilutes relevance.

---

## Phase 4: Verify

```bash
git log --oneline
gh repo view --web    # or open https://github.com/{username}/{repo-name}
```

Check:
- [ ] README renders correctly with no broken images
- [ ] Dark mode banner works (toggle GitHub theme in Settings)
- [ ] Topics are set
- [ ] Description is set

---

## Common Mistakes

**Using "Initial commit" as the commit message.**
The git log is visible on GitHub. "feat: initial release of {name}" takes 5 extra seconds and tells future visitors what was in the first commit.

**Initializing GitHub repo with a README.**
Checking "Add a README" on GitHub when you already have local content creates a diverged history. git will refuse to push. Always create an empty GitHub repo when local files are ready.

**Forgetting to set Topics before announcing the repo.**
Topics are the primary way GitHub search surfaces repos. A repo with no topics is invisible to most discovery paths. Set them immediately after pushing.

**Pushing to `master` instead of `main`.**
GitHub defaults new repos to `main`. If the local branch is `master`, run `git branch -M main` before pushing.

**Not reviewing `git status` before the first commit.**
A stray `git add .` can accidentally stage secrets, build artifacts, or system files. Always check `git status` output before committing.
