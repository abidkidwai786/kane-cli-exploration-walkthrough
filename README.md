# KaneCLI Exploration — a hands-on walkthrough

A worked, reproducible example of **TestMu AI KaneCLI *Exploration***: point an AI explorer at a live
web app, let it map the app and **design tests from what it finds**, then **pull those tests into this
folder and run them locally** — all from the command line.

If you've never seen KaneCLI before, this repo is a guided tour. Every command that was actually run is
listed in order, with the real output and the lessons learned along the way.

- **Explored app:** [Katalon CURA demo](https://katalon-demo-cura.herokuapp.com/) (a public sandbox)
- **Linked Test Manager project:** `Katalon-CURA-Explore` (`01M733DB8GHDVZV9JCSHVVV9AJ`)
- **kane-cli version used:** 0.10.2
- **Docs:** https://www.testmuai.com/docs/docs/kane-cli-exploration
- **Visual recap (Claude playbook):** https://claude.ai/artifact/7XVhEaTpLBaMgzzsJUT1B3

---

## Contents

1. [What is KaneCLI Exploration?](#1-what-is-kanecli-exploration)
2. [How it works (the lifecycle)](#2-how-it-works-the-lifecycle)
3. [Prerequisites](#3-prerequisites)
4. [Reproduce it yourself](#4-reproduce-it-yourself)
5. [Results & coverage](#5-results--coverage)
6. [Repo layout](#6-repo-layout)
7. [Lessons learned](#7-lessons-learned)

---

## 1. What is KaneCLI Exploration?

KaneCLI designs end-to-end tests in two ways. Both save into the same Test Manager project; the
difference is **where the requirements come from**:

| | **Exploration** (this repo) | **Assurance** |
|---|---|---|
| Starts from | a **live URL** | a **PRD / spec document** |
| Idea | *discover* what the app does | *derive* tests from the written spec |
| Produces | a map (screens, flows, findings) + designed tests | acceptance criteria → scenarios → tests |
| Use when | there's no spec, or you want to find gaps | "what exactly is covered, and how do we know?" |

> **Exploration designs tests but never runs them.** The explorer maps the app on the cloud
> (HyperExecute) and writes test cases; **you** pull them into a folder and run them.

---

## 2. How it works (the lifecycle)

```
  pick/create        explore            design jobs         context pull        testrun run         cover
  a project    ─▶   the live app  ─▶   map ➜ test cases ─▶  into a folder  ─▶   run locally   ─▶   coverage
 (1 proj = 1                             (automatic,          (downloads          (records, then      (vs discovered
  URL = 1 exp)                           on the cloud)        _test.md files)     replays)            use-cases)
```

---

## 3. Prerequisites

- **kane-cli** installed and signed in:
  ```bash
  npm install -g @testmuai/kane-cli
  kane-cli login --oauth
  ```
- **Google Chrome** (used for local `testrun` replays).
- A **TestMu AI account with credits** (exploration and the first run of each test consume credits).

> Every command below was run with a `KANE_CLI_USER_AGENT=claude-code` prefix (it just tags the
> runtime). It's omitted here for readability. On Windows in Git Bash, `testrun run` ends with
> `< /dev/null`; from `cmd.exe` use `< NUL`.

---

## 4. Reproduce it yourself

### Step 0 — Readiness check
```bash
kane-cli whoami            # signed in?
kane-cli balance           # enough credits?
kane-cli config show       # project, folder, default_url, target
```

### Step 1 — (What NOT to do) exploring a dead-end URL
The first attempt pointed at a single-screen onboarding **wizard**. It had nothing to navigate, so the
map came back empty and the design job failed with `INGEST_FAILED`. Re-running with a bigger budget and
a `mutating` policy did **not** help.

```bash
kane-cli projects create "KaneAI-Playground-Explore" --agent

kane-cli explore run \
  --project <playground-project-id> \
  --url https://kaneai-playground.lambdatest.io \
  --prompt "Explore the home page, navigation, forms, widgets" \
  --policy read_only --max-leases 20 --agent
```

> **Takeaway:** exploration needs an app with real surface area — links, menus, forms, multi-page flows.

### Step 2 — Explore a real multi-page app (Katalon CURA)
A fresh project, a login fed as a setup step (`--deps`), and that host allowed (`--allow-domain`).
Policy `mutating` is fine here because CURA is a throwaway public demo.

```bash
kane-cli projects create "Katalon-CURA-Explore" --agent
# -> project id 01M733DB8GHDVZV9JCSHVVV9AJ

kane-cli explore run \
  --project 01M733DB8GHDVZV9JCSHVVV9AJ \
  --url https://katalon-demo-cura.herokuapp.com/ \
  --prompt "Log in, then explore the Make Appointment form, submit an appointment, review the confirmation and history pages. Also explore the home page and menu." \
  --deps "Log in with username '{{login_username}}' and password '{{login_password}}'" \
  --allow-domain katalon-demo-cura.herokuapp.com \
  --policy mutating --max-leases 30 --agent
```

> The login uses the **public Katalon CURA demo account** (the credentials are printed on the CURA
> login page itself). Put them in your variables file — see [Step 5](#step-5--fill-in-variables).
>
> Result: **24 leases, 19 findings** — a real map. (The login setup didn't fully take, so the designed
> tests cover the public pages.)

### Step 3 — Watch the run and the design jobs
Exploration queues on the cloud and returns immediately. Poll until no design job is running or pending.

```bash
kane-cli explore sessions       <exploration-id> --agent   # run state, leases, findings
kane-cli explore context status <exploration-id> --agent   # what the design jobs saved
```

> Design jobs saved **6 Test Manager cases** (use-cases uc-1, uc-3, uc-4). One redesign of **uc-2** was
> held back by an internal citation check (`CITE_UNVERIFIED`) — nothing lost, but uc-2 has some
> undesigned criteria. Wait until `running_job` **and** `pending_cut` are both `null` before pulling.

### Step 4 — Pull the designed tests into this folder
This links the folder to the project and downloads the `_test.md` files.

```bash
kane-cli context pull --project 01M733DB8GHDVZV9JCSHVVV9AJ --mode agent
```

> Result: **11 tests** downloaded to `.testmuai/tests/`, plus 4 variables flagged as needed.

### Step 5 — Fill in variables
Copy the example and fill in the public CURA demo values. **The real file is git-ignored** so secrets
never get committed.

```bash
cp .testmuai/variables/assurance.example.json .testmuai/variables/assurance.json
# then edit assurance.json and set login_username / login_password
```

Validate they resolve before spending credits:
```bash
kane-cli testrun run ".testmuai/tests/verified-make-appointment-history-flow-reaches-history_test.md" \
  --dry-run --agent < /dev/null
# -> valid: true, no unresolved-variable warnings
```

### Step 6 — Run a test locally
The **first** run of a pulled test records its steps in a real browser (slow, ~30–85 credits); every
replay after that is cheap.

```bash
kane-cli testrun run ".testmuai/tests/verified-make-appointment-history-flow-reaches-history_test.md" \
  --agent < /dev/null
```

What happened here:
- **First run → `broken`.** The test drove login → Make Appointment screen successfully, then a
  *screenshot capture* timed out waiting for fonts. The evidence pack's auto-triage labelled it
  `environment_issue` — a flaky capture, **not** a product bug.
- **Retry → `passed`.** 4 acceptance criteria proven (ac-11, ac-12, ac-27, ac-28).

### Step 7 — Inspect evidence (optional)
Each run seals an evidence pack (a zip: logs, per-step screenshots, HAR, failure triage).

```bash
kane-cli evidence serve .testmuai/evidence/<evidence-id>.evidence   # opens in a browser
```

### Step 8 — Check coverage
Coverage is measured against the use-cases the exploration discovered, risk-weighted.

```bash
kane-cli cover --mode agent
```

To raise it, run the whole suite (clearing the **high-risk** uc-1 logout tests gives the biggest gain):
```bash
kane-cli testrun run .testmuai/tests/ --agent < /dev/null
```

---

## 5. Results & coverage

At the end of this activity: **10% overall** (4 of 38 acceptance criteria proven, from one passing run).

| Use-case | Risk | State | Proven |
|---|---|---|---|
| uc-3 · View appointment history | med | partial | 4 / 10 |
| uc-1 · Log out of the healthcare service | **high** | not run | 0 / 9 |
| uc-2 · Book a healthcare appointment | med | not run | 0 / 12 *(4 not designed)* |
| uc-4 · View a healthcare service profile | low | not run | 0 / 7 |

Only uc-3 is partially covered because just one of the 11 tests has been run. Running the full suite
raises the number; uc-2 can't reach 100% until its held-back use-case is redesigned.

---

## 6. Repo layout

```
Explore/
├─ README.md                              # this file
├─ .gitignore
└─ .testmuai/
   ├─ assurance.json                      # links this folder to the Test Manager project
   ├─ tests/
   │  └─ *_test.md                        # 11 designed tests (committed)
   └─ variables/
      └─ assurance.example.json           # template — copy to assurance.json and fill in

# git-ignored (not committed):
#   .testmuai/variables/assurance.json    # real credentials
#   .testmuai/evidence/                   # large sealed evidence packs (~29 MB)
#   .testmuai/tests/output-*/             # per-run output
```

---

## 7. Lessons learned

1. **One project = one exploration = one URL.** A different `--url` in an existing exploration project
   is refused — plan the project before the first run.
2. **Exploration needs surface area.** A single-screen wizard maps to nothing; pick a real multi-page app.
3. **Exploration designs tests; it never runs them.** Pull them, then run.
4. **The first run is the slow one.** It records steps in a live browser; replays are cheap.
5. **`broken` ≠ `failed`.** The evidence pack's triage tells you whether a red result is the app's
   fault or the test harness's — here it was a flaky screenshot, and a retry passed.
