# Plan: Extract `visual-prompt-coach` to its own public repo

## Context

The skill currently lives inside `wrsmith108/ResearchAgent` at `.claude/skills/visual-prompt-coach/`. That repo also contains the design docs in `docs/plans/` and a screenshot in `docs/screenshots/`, which is fine for an internal project but wrong for distribution. To install the skill, another user would have to clone the whole project and `cp -r` a subdirectory out of it.

The standard install path for Claude Code skills is `skillsmith import <github-owner>/<repo>`, which expects the skill files at the **root** of the target repo. So the skill needs its own repo.

## Goal

A standalone public repo at `https://github.com/wrsmith108/visual-prompt-coach` whose root contains `SKILL.md`, `frameworks.md`, `prompt-templates.md`, a real `README.md`, and a `LICENSE`. Others install with one command.

## Approach

Create a fresh local directory, copy the skill files out of ResearchAgent, rewrite the scaffolded README, push to a new public repo. No git history preservation — the current history is a single squash commit of a generated scaffold, not worth porting.

### Steps

1. **Pre-flight scan**
   - `skillsmith publish --check-references /Users/williamsmith/Documents/GitHub/ResearchAgent/.claude/skills/visual-prompt-coach/`
   - Fails the plan if any local paths (`/Users/williamsmith/...`, `../../../`) leak into the skill files. Fix before copying.

2. **Create the new local repo directory**
   - `mkdir /Users/williamsmith/Documents/GitHub/visual-prompt-coach`
   - Copy `SKILL.md`, `frameworks.md`, `prompt-templates.md` to the root.
   - Copy the `.gitignore` from the current skill folder.
   - Drop the scaffolded `scripts/` and `resources/` directories — the skill has no scripts or resources. Leaving empty dirs in a public repo is noise.

3. **Write a real `README.md`** (the scaffolded one is placeholders). Sections:
   - **Title + one-line description**
   - **What it does** — short paragraph pulled from `SKILL.md` § "What This Skill Does"
   - **Install**
     ```
     # Option 1 — skillsmith (recommended, user-level)
     skillsmith import wrsmith108/visual-prompt-coach

     # Option 2 — manual, user-level
     git clone https://github.com/wrsmith108/visual-prompt-coach \
       ~/.claude/skills/visual-prompt-coach

     # Option 3 — project-level
     git clone https://github.com/wrsmith108/visual-prompt-coach \
       .claude/skills/visual-prompt-coach
     ```
   - **Trigger phrases** — copied from `SKILL.md` § "When to Use"
   - **Example** — one realistic trigger and a short description of what comes back
   - **Frameworks applied** — bulleted list with one-line each (Dan Roam, C4, Mayer, CLT, Gestalt/CRAP)
   - **Files** — one-line per file explaining what's in it (so users can navigate)
   - **License** — MIT, link to LICENSE file

4. **Add `LICENSE`** — MIT text (the frontmatter already declares MIT; need the actual file for GitHub to recognize the license).

5. **Init, commit, push**
   - `git init -b main`
   - `git add .`
   - `git commit -m "Initial release: visual-prompt-coach v1.0.0"`
   - `gh repo create visual-prompt-coach --public --source=. --push --description "Claude Code skill — design visuals for technical course materials using Dan Roam, Mayer, C4, CLT, and Gestalt/CRAP"`

6. **Tag the release** — `git tag v1.0.0 && git push --tags`. Matches the `version: 1.0.0` declared in frontmatter and lets `skillsmith import` pin to a version.

7. **Decide what to do with the skill copy in ResearchAgent** *(open question — see below)*

## Target repo structure

```
visual-prompt-coach/
├── SKILL.md              # frontmatter + flow + intake + classification + output contract
├── frameworks.md         # Dan Roam, C4, Mayer, CLT, Gestalt+CRAP
├── prompt-templates.md   # Mermaid skeletons + Gemini/Nano Banana scaffolds
├── README.md             # rewritten for a public audience
├── LICENSE               # MIT
└── .gitignore
```

No `scripts/`, no `resources/`, no `docs/`, no nested `.claude/`.

## Open question — what happens to ResearchAgent?

Three options, pick one:

| Option | What happens | When it's right |
|---|---|---|
| **A. Leave as-is** | ResearchAgent still has the skill under `.claude/skills/`; the new repo is the canonical distribution. Minor duplication. | If you want ResearchAgent to stay runnable on its own without cloning the skill separately. |
| **B. Trim (recommended)** | Delete `.claude/skills/visual-prompt-coach/` from ResearchAgent and replace with a `README.md` in `.claude/skills/` that says "skill moved to wrsmith108/visual-prompt-coach". Design docs stay. | If you want ResearchAgent to remain the home for the design/plan docs, but not the skill source. |
| **C. Archive** | Delete or archive ResearchAgent; move `docs/plans/` into the new repo under `docs/`. | If ResearchAgent has no purpose once the skill is extracted. |

My recommendation: **B**. Keeps the design history where it belongs, removes the confusing duplication, and the new repo stays lean.

## Verification

1. `skillsmith validate /Users/williamsmith/Documents/GitHub/visual-prompt-coach/` → VALID.
2. `skillsmith publish --check-references /Users/williamsmith/Documents/GitHub/visual-prompt-coach/` → no leaked local paths.
3. `gh repo view wrsmith108/visual-prompt-coach --json visibility` → `"PUBLIC"`.
4. README renders correctly on github.com (no placeholder `<...>` slots left).
5. **Install smoke test** (in a scratch directory):
   ```
   skillsmith import wrsmith108/visual-prompt-coach
   ls ~/.claude/skills/visual-prompt-coach/SKILL.md
   ```
   Expect the file to exist. If `skillsmith import` fails for any reason, fall back to `git clone` and confirm that path works.
6. **Live trigger test** — new Claude Code session, say *"help me design a diagram for a lesson on OAuth flow"*, confirm the skill auto-loads from the user-level install.

## Out of scope for this plan

- Submitting to a public skills registry beyond GitHub.
- Adding CI (lint, validate on PR). Nice-to-have for v1.1.
- Multiple skills in one "skills collection" repo — this stays a single-skill repo.
