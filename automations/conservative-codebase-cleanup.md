# Conservative Codebase Cleanup Automation

## What it helps with

Removes proven dead code from one or more projects, verifies each change, and opens separate pull requests.

## Full prompt

```text
Run a conservative cleanup across all Git repositories in the open workspace roots. A user may narrow the scope with project names or paths, exclude projects, or request local-only delivery. Never search outside the chosen scope. Handle each repository independently.

For each repository:

1. Read `AGENTS.md`, `README.md`, relevant entry docs, and `~/.agents/skills/karpathy-guidelines/SKILL.md` when present. Inspect the code and Git state. Preserve unrelated user changes; skip overlapping work that cannot be isolated safely.
2. Remove only code, dependencies, tests, and assets proven unreachable, unreferenced, obsolete, or superseded. Keep anything ambiguous. Delete tests only when their production behavior no longer exists. Consolidate duplicates only when their contracts and behavior match. Follow existing patterns and avoid speculative refactors or broad churn.
3. Update `context/decisions.md` only when a lasting architectural decision changes. Run the repository's relevant tests, lint, type checks, build, and unused-code checks. Check the final diff for orphaned imports, files, assets, snapshots, dependencies, and scope drift. If verification fails, keep only independently valid changes and report the blocker.

If there are valid changes, create a unique `cleanup/` branch, commit only the cleanup without a `Co-Authored-By` trailer, push it, and open a non-draft pull request. Explain the evidence for each deletion or consolidation and list the checks run. Update an equivalent open cleanup PR when safe; do not create a duplicate. Never merge or deploy. In local-only mode, stop before branching or publishing.

If nothing warrants cleanup, make no changes, branch, commit, or PR and report: “Nothing to clean — the codebase is clean.”

Finish with a short report per repository: outcome, files changed, evidence, checks and results, blockers, and PR link. End with totals for clean, changed, skipped, and blocked repositories.
```

## Known limits

Runtime registration, generated references, plugins, and external consumers can hide real usage. Keep code when static evidence is inconclusive.

## Last tested

2026-08-03, with the Handwrite project.
