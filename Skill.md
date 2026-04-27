---
name: prd-agent
description: >
  Autonomous PRD (Product Requirements Document) Agent for open-source developers.
  Use this skill ANY TIME the user wants to: analyze a GitHub repo or local codebase for
  issues/bugs/improvements, generate GitHub-style issue markdown files (issue1.md, issue2.md, etc.),
  create structured PRDs or feature specs, triage problems in a project, or produce
  contribution-ready issue reports. Trigger when the user mentions "find issues", "repo analysis",
  "generate issues", "PRD", "open source contribution", "issue.md", or asks about problems in a codebase.
  This skill produces professional, actionable, contributor-ready issue documents.
---

# PRD Agent Skill

An autonomous agent that analyzes an open-source repository, identifies real issues
(bugs, missing features, tech debt, DX improvements, documentation gaps), and outputs
one structured **`issueN.md`** file per issue — ready to paste directly into GitHub Issues
or use as a contribution roadmap.

---

## Workflow

### Step 1 — Understand the Repo

Before generating any issues, gather context:

```
1. Read README.md, CONTRIBUTING.md, CHANGELOG.md (if present)
2. Scan directory tree (top 2 levels)
3. Check package.json / pyproject.toml / Cargo.toml / go.mod for deps & scripts
4. Skim key source files (entry points, core modules, config)
5. Check open GitHub Issues (if URL provided) to avoid duplicates
6. Look at recent commits / PR descriptions if accessible
```

Ask the user if they haven't provided:
- **Repo URL or local path** — required
- **Focus area** — (optional) e.g. performance, DX, security, docs, tests
- **Issue count** — how many issues to produce (default: 3–5)

---

### Step 2 — Identify Issues

Look for issues across these categories. Score each by **impact** (High/Med/Low) and **effort** (S/M/L).

| Category | What to look for |
|---|---|
| 🐛 **Bug** | Logic errors, unhandled edge cases, crashes, wrong behavior |
| ⚡ **Performance** | Unnecessary re-renders, N+1 queries, missing indexes, blocking I/O |
| 🔒 **Security** | Hardcoded secrets, missing auth checks, unsafe deserialization |
| 🧪 **Testing** | Missing tests, low coverage on critical paths, flaky tests |
| 📚 **Docs** | Missing JSDoc/typedoc, stale README sections, no examples |
| 🏗️ **Architecture** | Tight coupling, missing abstractions, config duplication |
| 🎨 **DX / UX** | Poor error messages, missing CLI flags, confusing APIs |
| 🔧 **Tooling** | Missing CI checks, no lint config, outdated deps |

---

### Step 3 — Generate Issue Files

For each identified issue, produce a file named `issue1.md`, `issue2.md`, etc.

Each file **must** follow the template in `references/issue-template.md`.

Key rules:
- Title must be **specific** — not "Fix bug in auth" but "JWT refresh token silently fails when `exp` is missing from payload"
- Include **reproduction steps** for bugs; **acceptance criteria** for features
- Link to the exact file + line number where relevant
- Suggest a concrete implementation approach (but don't over-prescribe)
- Keep language contributor-friendly — assume a capable but unfamiliar developer

---

### Step 4 — Prioritize & Summarize

After all issue files are written, produce a **`ISSUES_SUMMARY.md`** that:
- Lists all issues as a numbered table with Title, Category, Impact, Effort
- Recommends a **"good first issue"** label for low-effort/medium-impact ones
- Gives a 2-sentence strategic overview of the repo's health

---

## Output File Naming

```
issue1.md   ← highest priority / most impactful
issue2.md
issue3.md
...
ISSUES_SUMMARY.md
```

All files go to the project root (or wherever the user specifies).

---

## Quality Bar

Every issue file must pass this mental checklist before writing:

- [ ] Would a maintainer find this **actionable**?
- [ ] Is the **root cause** identified, not just the symptom?
- [ ] Does it have a **clear scope** (not too broad, not trivial)?
- [ ] Is the **implementation hint** realistic and helpful?
- [ ] Would a **new contributor** understand what to do?

If an issue doesn't pass, dig deeper or merge it with a related issue.

---

## Reference Files

- `references/issue-template.md` — The exact markdown template for each issue file
- `references/label-guide.md` — GitHub label taxonomy and when to apply each

Read these before writing any issue file.