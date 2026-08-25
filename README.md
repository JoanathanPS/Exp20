# LAB EX20: Create a GitHub Repository and Implement Version Control

Repo: https://github.com/JoanathanPS/Exp20

A small team-project simulation: two feature branches for two "modules", each
merged via a reviewed pull request, including one branch that hit a real merge
conflict which had to be resolved by hand.

## 1. Setup
```powershell
git clone https://github.com/JoanathanPS/Exp20.git
```
Repo was created with `auto_init` (starts with a README), then a shared
`main.py` entry point was committed to `main` first so both modules would
build on the same base.

## 2. Branches for individual modules
| Branch | Adds | Base |
|---|---|---|
| `module-a` | `moduleA.py` (user management) + edits the status line in `main.py` | tip of `main` |
| `module-b` | `moduleB.py` (task management) + edits the *same* status line in `main.py` | **older** commit of `main`, deliberately before `module-a` merged |

```powershell
git checkout -b module-a
# ...add moduleA.py, edit main.py...
git push -u origin module-a

git checkout main
git checkout -b module-b <commit-before-module-a>
# ...add moduleB.py, edit main.py (same line, different text)...
git push -u origin module-b
```

## 3. Pull requests + review
- **PR #1** `module-a` -> `main`: https://github.com/JoanathanPS/Exp20/pull/1
  Reviewed (comment left, since GitHub blocks self-approval on a solo repo),
  merged cleanly -- no conflict yet.
- **PR #2** `module-b` -> `main`: https://github.com/JoanathanPS/Exp20/pull/2
  Opened *after* PR #1 merged, so it now conflicts with `main` on the same
  line in `main.py`. GitHub reported `mergeable: false, mergeable_state: dirty`.

## 4. Conflict encountered + resolved
```powershell
git checkout module-b
git fetch origin
git merge origin/main
```
```
Auto-merging main.py
CONFLICT (content): Merge conflict in main.py
Automatic merge failed; fix conflicts and then commit the result.
```
`main.py` contained:
```python
<<<<<<< HEAD
    print(f"Welcome to {PROJECT_NAME} -- Module B (tasks) online")
=======
    print(f"Welcome to {PROJECT_NAME} -- Module A (users) online")
>>>>>>> origin/main
```
**Resolution:** kept both messages, combined into one status line:
```python
print(f"Welcome to {PROJECT_NAME} -- Module A (users) + Module B (tasks) online")
```
```powershell
git add main.py
git commit -m "Merge main into module-b and resolve conflict in main.py"
git push origin module-b
```
GitHub then reported the PR as mergeable again, and PR #2 was reviewed
and merged.

## 5. Final state
`main` now contains `main.py`, `moduleA.py`, and `moduleB.py`, with a
commit history showing two merge commits (one clean, one conflict-resolved).

## Screenshots to save here (1.png ... 5.png, max 5)
1. Repo's branches page showing `main`, `module-a`, `module-b`.
2. PR #1 (module-a) showing the review comment + merged status.
3. PR #2 (module-b) showing GitHub's "This branch has conflicts" warning.
4. Terminal output of the `git merge` conflict + resolution above.
5. PR #2 after resolution, showing merged status + the network/commit graph.
