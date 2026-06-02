---
name: git-flow
description: Guide a clean git workflow — branch naming, commit messages, rebase vs merge, PR checklist
---

You are helping with git workflow. The user may ask for help with branching, commit messages, resolving conflicts, PR prep, or history cleanup.

**BRANCH NAMING CONVENTION**
```
feat/short-description          — new feature
fix/issue-123-short-description — bug fix (include issue # if tracked)
docs/update-readme              — documentation only
chore/upgrade-dependencies      — maintenance, no behavior change
refactor/simplify-alerter       — code restructure, no behavior change
data/regenerate-synthetic       — data changes
test/add-forecaster-tests       — test additions
hotfix/critical-null-crash      — urgent production fix
```

**COMMIT MESSAGE FORMAT (Conventional Commits)**
```
<type>(<optional scope>): <short summary under 72 chars>

<optional body — explain WHY, not WHAT>

Co-Authored-By: Claude Sonnet 4.6 <noreply@anthropic.com>
```

Types: `feat` `fix` `docs` `chore` `refactor` `test` `data` `style` `perf`

Good: `feat(alerter): add auto-renew risk detection to renewal alerts`
Bad:  `updated stuff` / `fix bug` / `WIP`

**REBASE VS MERGE**
- `git rebase main` — use when your branch is behind main and you want a linear history. Safe for personal branches not shared with others.
- `git merge main` — use when the branch is shared (multiple contributors). Preserves divergence history.
- `git merge --squash` — use when you want one clean commit per feature in main. Good for PR merges.
- Never rebase a branch others have pulled.

**PR CHECKLIST**
Before opening a pull request:
```
□ git fetch origin && git rebase origin/main  (get up to date)
□ All tests pass locally
□ No debug print statements left in
□ No .env or secrets in the diff
□ Commit messages are clean (squash WIP commits if needed)
□ PR description explains WHY not WHAT
□ Screenshots/recordings for UI changes
□ CLAUDE.md updated if architecture changed
```

**COMMON TASKS**

Clean up last N commits before PR:
```bash
git rebase -i HEAD~N
# change 'pick' to 'squash' or 'fixup' for commits to merge
```

Undo last commit (keep changes staged):
```bash
git reset --soft HEAD~1
```

Fix commit message of last commit:
```bash
git commit --amend -m "corrected message"
```

Stash work in progress:
```bash
git stash push -m "WIP: description"
git stash pop  # restore later
```

Find which commit introduced a bug:
```bash
git bisect start
git bisect bad           # current commit is broken
git bisect good v1.0.0   # this tag was working
# git will check out commits for you to test
```

Respond to the user's specific git question using these principles. Always prefer safe, reversible operations. Flag any destructive commands (`--force`, `reset --hard`, `clean -f`) and confirm before suggesting them.
