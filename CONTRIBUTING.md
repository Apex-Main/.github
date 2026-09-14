# How we work in Apex-Main repositories

Nothing goes into `main` directly. Developers have read access to the Apex-Main repositories. You work in your own fork, open a pull request back to Apex-Main, and an approver reviews, approves and squash-merges it. This page explains the day-to-day.

## One-time setup per repository

1. On the repository page in Apex-Main, press **Fork** and fork it to your own account.
2. Clone your fork and point `upstream` at Apex-Main:
   ```
   git clone git@github.com:YOUR-USERNAME/REPO.git
   cd REPO
   git remote add upstream git@github.com:Apex-Main/REPO.git
   ```
   `origin` is your fork. `upstream` is Apex-Main. You push to origin, you pull from upstream.

## Starting a piece of work

1. **Get current.** Always start from Apex-Main's main, not your fork's:
   ```
   git checkout main
   git pull upstream main
   git push origin main
   ```
2. **Branch from main.** One branch per change, named for what it does:
   ```
   git checkout -b feature/xero-invoice-sync
   git checkout -b fix/stokly-stock-rounding
   git checkout -b hotfix/fodo-login-500
   ```
   Prefixes: `feature/`, `fix/`, `hotfix/`, `chore/`. Lower case, hyphens, no spaces.
3. **Commit small and often.** Push to your fork early: `git push -u origin feature/xero-invoice-sync`.

## Getting it into main

1. **Open a pull request** from your fork's branch to `Apex-Main/REPO` `main`. GitHub shows a "Compare & pull request" button right after you push. Fill in the template: what changed, why, how you tested it. Mark it Draft if it is not ready.
2. **Review is requested automatically** from the approvers via CODEOWNERS. You do not need to chase it, but a message on Teams saying the PR is up helps.
3. **If main has moved on**, bring your branch up to date and push again:
   ```
   git pull upstream main
   git push
   ```
   Fix conflicts on your branch, never on main.
4. **Address comments**, push again, resolve each thread. A new push means the approver will look again.
5. **An approver merges** with squash. Your PR becomes one commit on main. Delete the branch in your fork afterwards.

## When something goes wrong after a merge

- **Bad change is live:** open the merged PR and press **Revert**. That opens a new PR that undoes it. An approver merges it. Main is clean and history shows what happened.
- **Need to keep working on the fix:** revert first, then branch again from a freshly pulled main.
- **Never** try to rewrite or force-push `main`. You cannot, and that is the point.
- **Messed up your own branch?** It is in your fork. Reset it, rebase it, delete it and start again. Nothing in a fork can hurt Apex-Main until it is merged.

## Every day habits

- Pull `upstream main` before you branch. Pull again before you open the PR.
- One PR does one thing. Two unrelated fixes are two PRs.
- Write the PR for someone who was not there: what and why in the description, how to test in the checklist.
- Keep PRs to a few hundred lines where you can. Big ones wait longer.
- Secrets and client data never go in a commit, on any branch, in any fork.
