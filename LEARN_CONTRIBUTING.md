# 🎓 Complete Beginner's Guide to Open Source Contributing
### From Zero to Your First Merged Pull Request on Oppia

> **This guide is written for you — someone who knows a little Python theory,
> is new to Git, GitHub, open source, and this codebase, and wants to
> understand *everything* step by step.**
>
> Read it top to bottom. Every section builds on the last one.

---

## Table of Contents

1. [What is Open Source?](#1-what-is-open-source)
2. [Key Vocabulary: Issues, PRs, Forks, Merges](#2-key-vocabulary)
3. [How Git Works (the Basics)](#3-how-git-works)
4. [How GitHub Works](#4-how-github-works)
5. [What is Oppia?](#5-what-is-oppia)
6. [Codebase Tour: Where is Everything?](#6-codebase-tour)
7. [How the Pieces Connect (Backend → Frontend)](#7-how-the-pieces-connect)
8. [Local Setup — Step by Step](#8-local-setup)
9. [The Bug We Fixed](#9-the-bug-we-fixed)
10. [The Fix Explained in Plain English](#10-the-fix-explained)
11. [Understanding the Code Changes](#11-understanding-the-code-changes)
12. [How to Run the Tests](#12-how-to-run-the-tests)
13. [Proof That the Fix Works](#13-proof-that-the-fix-works)
14. [How to Write Your First Comment on an Issue](#14-how-to-write-your-first-comment)
15. [How to Push a PR and Get it Merged](#15-how-to-push-a-pr)
16. [What Happens After You Open a PR](#16-what-happens-after-you-open-a-pr)
17. [Cheat Sheet — All Commands in One Place](#17-cheat-sheet)
18. [Glossary of Terms](#18-glossary)

---

## 1. What is Open Source?

**Open source** means the source code (the actual program files) of a project
is available for anyone to read, download, suggest improvements to, or even
copy and build their own version from.

**Oppia** (https://www.oppia.org) is an open-source education platform. The
goal is to help anyone in the world learn anything interactively. The code lives
at https://github.com/oppia/oppia and anyone can contribute.

You found a fork of it at https://github.com/007rahulM/oppia — a **fork** is a
personal copy of the project inside your own GitHub account (more on that in
[section 4](#4-how-github-works)).

---

## 2. Key Vocabulary

Before writing a single line of code you need to speak the language.

| Word | Plain English meaning |
|------|-----------------------|
| **Repository (Repo)** | A folder that contains all the project files AND the complete history of every change ever made to them. |
| **Issue** | A problem report or feature request written on GitHub. Think of it as a bug ticket or a to-do item. |
| **Fork** | Your personal copy of someone else's repo. Changes to your fork don't affect the original. |
| **Branch** | A parallel version of the code inside a repo. You create a branch to work on a feature without touching the stable code. |
| **Commit** | A saved snapshot of your changes. Like pressing "Save" in a document but with a description of what you changed. |
| **Push** | Uploading your local commits to GitHub (the remote copy). |
| **Pull Request (PR)** | A request you send saying "please look at my changes and merge them into the main project." |
| **Merge** | Taking the changes from one branch and combining them into another branch. |
| **Review** | A maintainer reading your PR and leaving comments or approving it. |
| **CI / Tests** | Automated checks that run when you open a PR to make sure you haven't broken anything. CI = Continuous Integration. |
| **Clone** | Downloading a repo from GitHub to your computer. |
| **Upstream** | The original repo (oppia/oppia). Your fork is **downstream** from it. |
| **Origin** | Your fork on GitHub. |

---

## 3. How Git Works

Git is a **version control system** — software that tracks every change to every
file in a project over time.

### The Three Areas

```
Your text editor  →  Staging area  →  Local repo  →  GitHub (remote)
   (working dir)        (git add)     (git commit)    (git push)
```

- **Working directory** — files on your hard drive that you are editing right now.
- **Staging area** — a list of files you have marked ("staged") to be included
  in the next commit.
- **Local repo** — the `.git` folder inside your project that stores all commits.
- **Remote (GitHub)** — the copy of the repo hosted online.

### Essential Git Commands

```bash
# 1. Download a repo to your computer
git clone https://github.com/007rahulM/oppia.git

# 2. See what files you have changed
git status

# 3. See exactly what changed inside files
git diff

# 4. Stage a file for committing
git add path/to/file.py

# 5. Stage ALL changed files
git add .

# 6. Save a commit (snapshot) with a message
git commit -m "Fix: use 127.0.0.1 instead of localhost in task handler"

# 7. Upload your commits to GitHub
git push origin your-branch-name

# 8. Download the latest changes from upstream
git fetch upstream
git merge upstream/develop

# 9. Create a new branch
git checkout -b fix/cron-localhost-issue

# 10. Switch to an existing branch
git checkout main

# 11. View all branches
git branch -a

# 12. View commit history
git log --oneline -10
```

### Why Branches?

Imagine a river (the `develop` or `main` branch — the stable code). When you
want to add a bridge, you don't tear up the river. You work on a separate
**tributary branch**, build the bridge, test it, and only then merge it into
the main river.

```
main:   ──A──B──C──────────────M──
                 \             /
your-branch:      D──E──F──G──
```

- `A`, `B`, `C` = existing commits on main
- `D`–`G` = your commits on your branch
- `M` = the merge commit (your work is now in main)

---

## 4. How GitHub Works

GitHub is a website that hosts Git repositories and adds collaboration tools on
top:

- **Issues tab** — where bugs and feature requests are tracked.
- **Pull Requests tab** — where you propose changes.
- **Actions tab** — shows automated CI test results.
- **Fork button** — creates your own copy of a repo.

### The Contribution Flow

```
oppia/oppia  ←── (upstream original)
     │
     │ Fork (once, on GitHub website)
     ▼
007rahulM/oppia  ←── (your fork on GitHub)
     │
     │ Clone (once, on your computer)
     ▼
~/oppia  ←── (your local copy)
     │
     │ Create branch, make changes, commit
     │ Push branch to your fork
     ▼
007rahulM/oppia  ←── (your fork, now with your branch)
     │
     │ Open Pull Request (on GitHub website)
     ▼
oppia/oppia  ←── (maintainers review and merge)
```

---

## 5. What is Oppia?

Oppia is a **web application** — software that runs in a browser.

- **Backend** — Python code running on Google App Engine (a cloud platform).
  It handles user data, explorations (interactive lessons), emails, scheduled
  tasks (cron jobs), etc.
- **Frontend** — Angular + TypeScript code that runs in the user's browser.
  It shows the UI — lesson pages, dashboards, profile pages, etc.
- **Database** — Google Cloud Datastore (a NoSQL database in the cloud) and
  Redis (in-memory cache).

When you visit `https://www.oppia.org`, your browser loads the Angular app,
which talks to the Python backend via HTTP calls (like API calls).

---

## 6. Codebase Tour

Here are the most important folders and what they do:

```
oppia/
├── core/
│   ├── controllers/       ← Python: URL handlers (what happens when a URL is called)
│   │   ├── base.py        ← The base class every handler inherits from
│   │   ├── cron.py        ← Handlers for scheduled (cron) jobs
│   │   └── acl_decorators.py  ← Access control (who can do what)
│   │
│   ├── domain/            ← Python: Business logic (the real rules of the app)
│   │   ├── cron_services.py   ← Logic for cleaning up old data
│   │   └── taskqueue_services.py  ← Logic for deferring work to a queue
│   │
│   ├── platform/          ← Python: Platform-specific code (dev vs. production)
│   │   └── taskqueue/
│   │       ├── dev_mode_taskqueue_services.py   ← ⭐ WE CHANGED THIS FILE
│   │       └── cloud_taskqueue_services.py      ← Used in production
│   │
│   ├── storage/           ← Python: Database model definitions
│   │   └── cloud_task/
│   │       └── gae_models.py  ← CloudTaskRunModel
│   │
│   └── templates/         ← Angular/TypeScript: Frontend UI
│
├── scripts/
│   ├── start.py           ← ⭐ WE CHANGED THIS FILE — starts the dev server
│   └── servers.py         ← Helper functions to manage server processes
│
├── cron.yaml              ← Defines which URLs Google runs on a schedule
├── main.py                ← URL routing (which URL → which handler)
└── app_dev.yaml           ← Development server configuration
```

### The `_test.py` Pattern

Every Python file has a matching test file. For example:

| Source file | Test file |
|-------------|-----------|
| `dev_mode_taskqueue_services.py` | `dev_mode_taskqueue_services_test.py` |
| `start.py` | `start_test.py` |
| `cron.py` | `cron_test.py` |
| `acl_decorators.py` | `acl_decorators_test.py` |

Tests live right next to the code they test.

---

## 7. How the Pieces Connect

### A Cron Job Request, Step by Step

When a cron job is triggered (either by Google's scheduler or manually from the
admin page at `http://localhost:8000/cron`), here is exactly what happens:

```
Browser/Scheduler
      │  HTTP GET /cron/models/cleanup
      ▼
main.py            ← URL router — finds the right handler
      │
      ▼
core/controllers/cron.py
  CronModelsCleanupHandler.get()
      │
      │  @acl_decorators.can_perform_cron_tasks  ← checks X-AppEngine-Cron header
      ▼
core/domain/cron_services.py
  delete_models_marked_as_deleted()
  mark_outdated_models_as_deleted()
      │
      ▼
core/storage/  ← reads/writes to the database
```

### A Deferred Task (like user deletion)

Some cron handlers don't do work directly — they put work onto a **task queue**
so it can run asynchronously (in the background):

```
CronUserDeletionHandler.get()
      │
      ▼
taskqueue_services.defer(fn, queue_name)
      │
      ▼
dev_mode_taskqueue_services.create_http_task()
      │
      ▼
cloud_tasks_emulator.Emulator (a background thread)
      │
      │  HTTP POST http://127.0.0.1:8181/task/deferred  ← ⭐ THIS IS WHERE THE BUG WAS
      ▼
core/controllers/tasks.py
  DeferredTasksHandler.post()
```

---

## 8. Local Setup

### What You Need (Prerequisites)

| Tool | Why | How to check |
|------|-----|--------------|
| Git | Version control | `git --version` |
| Python 3.9+ | Backend language | `python3 --version` |
| Node.js 16+ | Frontend build | `node --version` |
| Chrome browser | For acceptance tests | open Chrome |
| Linux or macOS | Oppia doesn't officially support Windows natively | `uname -s` |

> **Windows users**: Use WSL2 (Windows Subsystem for Linux) — install Ubuntu
> from the Microsoft Store, then follow the Linux instructions inside WSL2.

### Step 1: Fork the Repo

1. Go to https://github.com/oppia/oppia
2. Click the **Fork** button (top right)
3. Select your account → creates `https://github.com/YOUR_USERNAME/oppia`

### Step 2: Clone Your Fork

```bash
# Replace YOUR_USERNAME with your GitHub username
git clone https://github.com/YOUR_USERNAME/oppia.git
cd oppia
```

### Step 3: Add the Upstream Remote

```bash
# This lets you pull in new changes from the main Oppia repo
git remote add upstream https://github.com/oppia/oppia.git

# Verify
git remote -v
# Should show:
# origin    https://github.com/YOUR_USERNAME/oppia.git (fetch)
# origin    https://github.com/YOUR_USERNAME/oppia.git (push)
# upstream  https://github.com/oppia/oppia.git (fetch)
# upstream  https://github.com/oppia/oppia.git (push)
```

### Step 4: Create a Python Virtual Environment

```bash
python3 -m venv venv
source venv/bin/activate   # On Windows (WSL): same command
```

### Step 5: Install Third-Party Libraries

```bash
python -m scripts.install_third_party_libs
```

This takes 10–20 minutes the first time. It downloads all Python and JavaScript
dependencies.

### Step 6: Start the Development Server

```bash
python -m scripts.start
```

This starts:
- The Google App Engine dev server on port `8181`
- The GAE admin server on port `8000` (where you trigger cron jobs)
- Redis (cache) on port `6379`
- Elasticsearch (search) on port `9200`
- Firebase auth emulator on port `9099`
- The Angular dev server (webpack) watching for frontend changes

After a minute or two you will see:

```
INFORMATION
Local development server is ready! You can access it by
navigating to http://localhost:8181/ in a web browser.
```

Open http://localhost:8181/ in Chrome.

### Step 7: Verify Cron Jobs Work

After the fix in this PR:

1. Open http://localhost:8000/cron (the GAE admin panel)
2. Click the ▶ "Run now" button next to any cron job
3. You should see a `200 OK` response — the job ran successfully ✅

Without the fix you would see a connection error or a `500` response ❌

---

## 9. The Bug We Fixed

### Issue #25522 — Cron Jobs Fail on `localhost` but Work on `0.0.0.0`

**The report (simplified):**

> When I go to `http://localhost:8000/cron` and click "Run now" on any cron job,
> it fails. But if I go to `http://0.0.0.0:8000/cron` it works fine.
>
> — @mosin74

### Why Does This Happen?

The key concept to understand is **IPv4 vs IPv6**:

- **IPv4** addresses look like `192.168.1.1` or `127.0.0.1`
- **IPv6** addresses look like `::1` (the loopback address, equivalent to
  `127.0.0.1` in IPv6)
- **`localhost`** is a hostname. On modern Linux (Ubuntu 20.04+, Arch, etc.)
  `localhost` often resolves to **`::1` (IPv6)** first.
- **`127.0.0.1`** is always IPv4.
- **`0.0.0.0`** means "all IPv4 interfaces" — it only works with IPv4.

### The Problem Visualised

```
  Before the fix:

  Browser accesses localhost:8000/cron
       │
       ▼
  GAE admin server triggers a cron job
       │
       │  HTTP POST to "localhost:8181/cron/models/cleanup"
       │
       │  On Ubuntu: localhost → ::1 (IPv6)
       ▼
  Dev app server is listening on 0.0.0.0 (IPv4 ONLY!)
       │
       │  Connection refused! IPv6 ::1 ≠ IPv4 0.0.0.0
       ▼
  Cron job FAILS ❌

  ─────────────────────────────────────────────────

  Browser accesses 0.0.0.0:8000/cron
       │
       ▼
  GAE admin server triggers a cron job
       │
       │  HTTP POST to "0.0.0.0:8181/cron/models/cleanup"
       │
       │  0.0.0.0 → 127.0.0.1 (IPv4)
       ▼
  Dev app server is listening on 0.0.0.0 (IPv4) ✅
       │
       │  Connection succeeds!
       ▼
  Cron job WORKS ✅
```

### The Same Problem in the Task Queue

The `dev_mode_taskqueue_services.py` file was building internal HTTP request
URLs like this:

```python
# BEFORE the fix — BROKEN on some Linux systems
complete_url = 'http://localhost:%s%s' % (GOOGLE_APP_ENGINE_PORT, url)
```

When `localhost` resolves to `::1` (IPv6) but the server only listens on
`0.0.0.0` (IPv4), this request also fails.

---

## 10. The Fix Explained

We made **two fixes**:

### Fix 1 — Task Queue URL

**File:** `core/platform/taskqueue/dev_mode_taskqueue_services.py`

```python
# BEFORE (broken)
complete_url = 'http://localhost:%s%s' % (GOOGLE_APP_ENGINE_PORT, url)

# AFTER (fixed)
complete_url = 'http://127.0.0.1:%s%s' % (GOOGLE_APP_ENGINE_PORT, url)
```

**Why this works:** `127.0.0.1` is *always* IPv4, regardless of how your
operating system resolves `localhost`. No ambiguity.

### Fix 2 — Dev Server Bind Address

**File:** `scripts/start.py`

```python
# BEFORE — server binds to 0.0.0.0 by default (all IPv4 interfaces)
servers.managed_dev_appserver(
    app_yaml_path,
    enable_host_checking=not parsed_args.disable_host_checking,
    # no `host` argument — default was 0.0.0.0
    ...
)

# AFTER — server explicitly binds to 127.0.0.1 for local dev
servers.managed_dev_appserver(
    app_yaml_path,
    enable_host_checking=not parsed_args.disable_host_checking,
    host='0.0.0.0' if parsed_args.disable_host_checking else '127.0.0.1',
    ...
)
```

**Why this works:**
- By binding the app server to `127.0.0.1`, both the browser's `localhost:8000`
  request *and* the internal cron-trigger request go to the same IPv4 address.
- When you use `--disable_host_checking` (to share the server on your local
  network), we fall back to `0.0.0.0` so the server is reachable from other
  devices.

---

## 11. Understanding the Code Changes

Let's read the four changed files together:

### File 1: `core/platform/taskqueue/dev_mode_taskqueue_services.py`

This file is the **development-mode task queue**. In production, Oppia uses
Google Cloud Tasks (a paid cloud service). In development on your laptop, it
uses this file instead — it's a local emulator.

The function `_task_handler` is called every time a background task needs to
run. It makes an HTTP POST request to the app server to actually execute the
task.

```python
def _task_handler(url, payload, queue_name, task_name=None):
    headers = {}
    headers['X-Appengine-QueueName'] = queue_name
    # ... more headers ...
    headers['X-AppEngine-Fake-Is-Admin'] = '1'   # ← fakes admin auth for dev

    # Build the full URL: http://127.0.0.1:8181/task/deferred
    complete_url = 'http://127.0.0.1:%s%s' % (GOOGLE_APP_ENGINE_PORT, url)

    # Make the HTTP request
    requests.post(complete_url, json=payload, headers=headers, ...)
```

### File 2: `core/platform/taskqueue/dev_mode_taskqueue_services_test.py`

The test verifies that `_task_handler` builds the right URL. Since we changed
`localhost` → `127.0.0.1`, the test expectation had to change too:

```python
def test_task_handler_will_create_the_correct_post_request(self):
    def mock_post(url, json, headers, timeout):
        self.assertEqual(
            url, 'http://127.0.0.1:%s%s' % (correct_port, dummy_url)
            #          ^^^^^^^^^ was localhost
        )
    ...
```

### File 3: `scripts/start.py`

This is the **entry point script** you run to start the whole dev environment.
The change adds one line:

```python
host='0.0.0.0' if parsed_args.disable_host_checking else '127.0.0.1',
```

`parsed_args.disable_host_checking` is `True` when you run:

```bash
python -m scripts.start --disable_host_checking
```

That flag is meant for sharing the server on a local network (e.g., testing on
your phone). Without the flag, we now explicitly bind to `127.0.0.1` so cron
jobs always use IPv4.

### File 4: `scripts/start_test.py`

Two test changes:

1. The existing test for `--disable_host_checking` was updated to assert
   `host='0.0.0.0'`.
2. A **new test** was added to verify the default case uses `host='127.0.0.1'`:

```python
def test_main_uses_localhost_as_host_by_default(self):
    start.main(['--no_browser', '--skip_install'])
    self.mock_dev_appserver.assert_called_once_with(
        'app_dev.yaml',
        enable_host_checking=True,
        host='127.0.0.1',          # ← new assertion
        automatic_restart=True,
        skip_sdk_update_check=True,
        port=8181,
        env=mock.ANY,
    )
```

---

## 12. How to Run the Tests

Oppia uses Python's `unittest` framework, run via a custom script.

### Run a Specific Test File

```bash
python -m scripts.run_backend_tests \
    --test_target core.platform.taskqueue.dev_mode_taskqueue_services_test
```

### Run Tests for the Start Script

```bash
python -m scripts.run_backend_tests \
    --test_target scripts.start_test
```

### What Success Looks Like

```
.....
----------------------------------------------------------------------
Ran 5 tests in 0.123s

OK
```

### What Failure Looks Like

```
FAIL: test_task_handler_will_create_the_correct_post_request
----------------------------------------------------------------------
AssertionError: 'http://localhost:8181/dummy_handler' !=
                'http://127.0.0.1:8181/dummy_handler'
```

If you see `OK` — your change is safe. If you see `FAIL` — something broke and
you need to investigate.

---

## 13. Proof That the Fix Works

Here is how to demonstrate the fix on your local machine:

### Before the Fix (to reproduce the bug)

1. Revert the change temporarily:
   ```bash
   git stash    # saves your changes temporarily
   python -m scripts.start
   ```
2. Open http://localhost:8000/cron
3. Click "Run now" on "weekly cleanup of models marked as deleted"
4. Check the logs — you'll see a `ConnectionRefused` or `500` error ❌

5. Now open http://0.0.0.0:8000/cron
6. Click the same job — it succeeds ✅

7. Restore the fix:
   ```bash
   git stash pop
   ```

### After the Fix

1. Start the server:
   ```bash
   python -m scripts.start
   ```
2. Open http://localhost:8000/cron
3. Click "Run now" — SUCCESS ✅ (you should see a `200` response)

### Screenshot Evidence

When you submit your PR, the maintainers will ask for a demo. You should:

1. Open http://localhost:8000/cron in Chrome
2. Open Chrome DevTools → Network tab (F12)
3. Click "Run now" on a cron job
4. Take a screenshot showing the `200 OK` response
5. Attach it to your GitHub comment/PR description

---

## 14. How to Write Your First Comment on an Issue

GitHub issues are like a discussion thread. The maintainers expect certain
things from contributors. Here is a template:

---

### Comment Template — "I'm Working on This"

> Hi! I've been looking into this bug.
>
> **Root cause:**
> On modern Linux systems, `localhost` can resolve to `::1` (IPv6) instead of
> `127.0.0.1` (IPv4). The development server only binds to IPv4, so cron job
> requests from the admin UI at `localhost:8000/cron` fail because the internal
> connection can't be established.
>
> **Fix:**
> 1. In `dev_mode_taskqueue_services.py`, replace `localhost` with `127.0.0.1`
>    in the task handler URL so internal task execution always uses IPv4.
> 2. In `scripts/start.py`, explicitly bind the dev server to `127.0.0.1`
>    (IPv4 loopback) when running locally, which ensures the GAE admin UI's
>    cron trigger requests always reach the app server correctly.
>
> I've verified this locally — cron jobs now work when triggered from
> `localhost:8000/cron`. I'll open a PR shortly.
>
> **PR link:** (add when you open it)

---

### Rules for Good GitHub Comments

- Be **polite and professional** — real humans are reading this.
- **Link evidence** — screenshots, logs, PR links.
- **Be concise** — maintainers read dozens of issues per day.
- **Don't say "I'll fix this" without explaining how** — show you understand
  the bug before claiming it.
- **Use Markdown** — GitHub renders it nicely (bold with `**`, code with
  backticks, etc.).

---

## 15. How to Push a PR

### Step 1: Make Sure You're on the Right Branch

```bash
git branch    # shows current branch, marked with *
```

The fix lives on branch `copilot/resolve-issue-and-teach-git` in the fork
`007rahulM/oppia`. For your own fix you would create a new branch:

```bash
git checkout -b fix/cron-localhost-issue
```

### Step 2: Make Your Changes

Edit files in your text editor (VS Code, PyCharm, etc.).

### Step 3: Stage and Commit

```bash
git add core/platform/taskqueue/dev_mode_taskqueue_services.py
git add core/platform/taskqueue/dev_mode_taskqueue_services_test.py
git add scripts/start.py
git add scripts/start_test.py

git commit -m "Fix: use 127.0.0.1 instead of localhost to fix cron jobs on Linux"
```

### Step 4: Push to Your Fork

```bash
git push origin fix/cron-localhost-issue
```

### Step 5: Open a Pull Request on GitHub

1. Go to https://github.com/007rahulM/oppia
2. You'll see a yellow banner: *"fix/cron-localhost-issue had recent pushes"*
   → Click **Compare & pull request**
3. Set:
   - **Base repository:** `oppia/oppia` (the upstream)
   - **Base branch:** `develop`
   - **Head repository:** `007rahulM/oppia`
   - **Head branch:** `fix/cron-localhost-issue`
4. Write the PR description (see template below)
5. Click **Create pull request**

### PR Description Template

```
## What's the problem?
Cron jobs triggered from the GAE admin UI at `http://localhost:8000/cron`
were failing with a connection error. The same jobs succeeded when triggered
from `http://0.0.0.0:8000/cron`.

## What's the root cause?
On modern Linux (Ubuntu 22.04+), `localhost` resolves to `::1` (IPv6 loopback)
rather than `127.0.0.1` (IPv4). The development server only listens on IPv4,
so IPv6 connections are refused.

Two code paths were affected:
1. `dev_mode_taskqueue_services._task_handler` built internal HTTP request
   URLs using `localhost`.
2. `scripts/start.py` started the dev server bound to `0.0.0.0` (all IPv4
   interfaces), which combined with the GAE admin UI's localhost routing
   caused failures.

## What did I change?
- `dev_mode_taskqueue_services.py`: Changed `localhost` → `127.0.0.1`
- `scripts/start.py`: Explicitly pass `host='127.0.0.1'` for local dev,
  `host='0.0.0.0'` for `--disable_host_checking` mode

## Tests
- Updated `dev_mode_taskqueue_services_test.py` URL expectation
- Updated existing test in `start_test.py` for `--disable_host_checking`
- Added new test `test_main_uses_localhost_as_host_by_default`

## Screenshot / Demo
[attach screenshot of localhost:8000/cron showing 200 OK after the fix]

Fixes #25522
```

---

## 16. What Happens After You Open a PR

1. **CI runs automatically** — GitHub Actions runs all the backend tests,
   frontend tests, linting, and type checks. This takes 20–40 minutes.
   - Green checkmarks ✅ = tests pass
   - Red X ❌ = something broke, you need to fix it

2. **A reviewer is assigned** — an Oppia maintainer will read your code and
   leave comments. Be patient — this can take days or weeks.

3. **Address review comments** — make the requested changes, commit them, and
   push. The PR updates automatically.

4. **LGTM (Looks Good To Me)** — when the reviewer approves, they add this.

5. **Merge** — a maintainer merges your PR into `develop`. Your code is now
   part of the official Oppia codebase! 🎉

### Common Review Requests

| What they'll say | What it means |
|-----------------|---------------|
| "Add a docstring" | Add a `"""..."""` comment explaining what a function does |
| "This needs a test" | Write a unit test for the new behavior |
| "Nit: ..." | "Nitpick" — a small style suggestion, usually optional |
| "Please rebase" | Update your branch with the latest upstream changes |
| "LGTM" | They approve — wait for a final merge |

---

## 17. Cheat Sheet — All Commands in One Place

```bash
# ── SETUP ──────────────────────────────────────────────────────────────────
git clone https://github.com/YOUR_USERNAME/oppia.git
cd oppia
git remote add upstream https://github.com/oppia/oppia.git
python -m venv venv && source venv/bin/activate
python -m scripts.install_third_party_libs

# ── DAILY WORKFLOW ─────────────────────────────────────────────────────────
git fetch upstream
git merge upstream/develop          # keep your fork up to date
git checkout -b fix/my-branch-name  # create a branch for your change
# ... make changes ...
git add .
git commit -m "Fix: description of what you did"
git push origin fix/my-branch-name
# → open PR on GitHub

# ── TESTING ────────────────────────────────────────────────────────────────
python -m scripts.run_backend_tests \
    --test_target core.platform.taskqueue.dev_mode_taskqueue_services_test
python -m scripts.run_backend_tests \
    --test_target scripts.start_test

# ── RUN THE SERVER ─────────────────────────────────────────────────────────
python -m scripts.start
# Visit: http://localhost:8181   (app)
# Visit: http://localhost:8000   (admin)
# Visit: http://localhost:8000/cron  (trigger cron jobs)

# ── UNDO A MISTAKE ─────────────────────────────────────────────────────────
git checkout -- path/to/file.py     # discard changes to one file
git stash                           # hide all changes temporarily
git stash pop                       # restore stashed changes

# ── CHECK WHAT'S HAPPENING ─────────────────────────────────────────────────
git status                          # what files changed?
git diff                            # what exactly changed?
git log --oneline -10               # last 10 commits
git branch -a                       # all branches
```

---

## 18. Glossary

| Term | Meaning |
|------|---------|
| `IPv4` | Internet Protocol version 4. Address format: `127.0.0.1` |
| `IPv6` | Internet Protocol version 6. Address format: `::1` |
| `localhost` | A hostname that points to your own computer. Resolves to `127.0.0.1` (IPv4) or `::1` (IPv6) depending on your OS |
| `127.0.0.1` | The IPv4 loopback address — always means "this machine" |
| `0.0.0.0` | "All IPv4 interfaces" — when a server binds to this, it accepts connections on all network cards |
| `Cron job` | A task scheduled to run automatically at a specific time |
| `Cron.yaml` | Oppia's config file that defines which URLs to call and when |
| `Task queue` | A list of background jobs to run asynchronously |
| `Dev server` | A local server (Google App Engine dev server) that mimics production on your laptop |
| `GAE` | Google App Engine — the cloud platform Oppia runs on |
| `HTTP GET` | A web request that reads data from a URL |
| `HTTP POST` | A web request that sends data to a URL (triggers an action) |
| `200 OK` | An HTTP status code meaning "success" |
| `500` | An HTTP status code meaning "server error" |
| `Header` | Extra metadata sent with an HTTP request (like `X-AppEngine-Cron: true`) |
| `Decorator` | In Python, an `@something` above a function that wraps it with extra behavior |
| `ACL` | Access Control List — rules about who can do what |
| `Unit test` | A test that checks one small piece of logic in isolation |
| `Mock` | A fake version of a function/object used in tests so you don't need real external services |
| `CI` | Continuous Integration — automated tests that run on every push/PR |
| `LGTM` | "Looks Good To Me" — code review approval phrase |
| `Nit` | "Nitpick" — a tiny optional code style suggestion in a review |
| `Rebase` | Rewriting your branch's history to sit on top of the latest upstream commits |
| `Stash` | A temporary holding area for uncommitted changes |

---

## ✅ Summary: What We Did

1. **Found the bug**: Cron jobs fail on `localhost:8000/cron` because `localhost`
   resolves to IPv6 (`::1`) on some Linux systems, but the dev server only
   listens on IPv4.

2. **Fixed it in two places**:
   - Changed `localhost` → `127.0.0.1` in the task handler URL
     (`dev_mode_taskqueue_services.py`)
   - Explicitly bind the dev server to `127.0.0.1` when running locally
     (`scripts/start.py`)

3. **Updated the tests** to reflect the new behavior (two files updated,
   one new test added).

4. **Opened a PR** with a clear explanation, linking to the issue (#25522).

---

*This guide was written to help you understand open source contribution from
the ground up. Every term, every command, every concept is explained. Read it,
try the commands, and don't be afraid to make mistakes — that's how you learn.*

*Good luck! 🚀*
