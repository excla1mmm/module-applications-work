# Protecting the default branch

The checks only colour a pull request; without branch protection anyone with write access can merge a red one
(seen in the sandbox on 2026-10-27: an edit to `test.yaml` and a module that failed its tests were merged). The
rules below make the checks a gate. Set them as a ruleset (Settings → Rules → Rulesets → New branch ruleset) on the
default branch.

| Setting | Value | Why |
|---|---|---|
| Enforcement | Active | |
| Bypass list | empty (or option A below for the image update) | owners and admins go through the same gate (LIC-614: "the bypass rights are accounted for") |
| Restrict deletions, Block force pushes | on | the registry's history is the record |
| Require a pull request before merging | on, 1 approval | MIKO's decision is a person's |
| Dismiss stale pull request approvals when new commits are pushed | on | an approval covers one commit and one revision of the application |
| Require review from Code Owners | on once CODEOWNERS names MIKO's reviewers | |
| Require approval of the most recent reviewable push | on | the last push is approved by someone other than its author |
| Require status checks to pass | on, "Require branches to be up to date before merging" on | results must be for the commit being merged, on the current base |
| Required checks | `result` (Test), `gather` (Validate), `terms-accepted` (Terms), `agreement` (Publisher agreement), `AI review` (commit status) | see below |

What each required check means:

- **`result`** — the application's tests passed on every declared architecture; or the pull request is maintenance
  by the owners or members that changes no application.
- **`gather`** — the application follows the rules (`tools/validate.py --online`).
- **`terms-accepted`** — the terms box is ticked (owners and members are exempt).
- **`agreement`** — every commit author signed the publisher agreement. Maintainers sign once; until the text is
  approved by MIKO this is the pilot's draft.
- **`AI review`** — set by `ai-review.yaml` on the tested head: success only for a complete review without blockers,
  or for a maintenance pull request with no application (checked through the API). It runs after a person approves
  the `ai-review` environment, so the merge waits for that too. A new push leaves the new head without it.

## The image update pull request

`image-update.yaml` opens its pull request with GITHUB_TOKEN, and GitHub starts no workflow for such a pull request,
so the required checks never appear on it and the ruleset blocks it. The control modules have already run in the
image update's own run (linked from the pull request). Two ways out, MIKO's choice:

- **A. Admin bypass for pull requests only** (pilot): in the ruleset's bypass list add *Repository admin*, mode
  *For pull requests only*. Direct pushes stay blocked; an admin can merge a pull request without its checks, and
  GitHub records each bypass. By convention only image update pull requests are merged this way. Weaker: the
  convention is not enforced.
- **B. A GitHub App for the image update**: the workflow opens the pull request with the App's token (stored as a
  secret in its own environment, `image-update`), so the normal checks run on it and no bypass is needed. Stronger,
  but one more credential to own.

Also: Settings → Actions → General → "Allow GitHub Actions to create and approve pull requests" on (needed for A).

## In the sandbox (2026-10-27)

Ruleset `main: applications gate` (id 25947319) on excla1mmm/module-applications follows the table, option A, with
two differences for a one-person sandbox: "Require approval of the most recent reviewable push" is **off** (turn it
on in the real repository) and code owner review is off (no CODEOWNERS yet).

Checked without using the bypass:

- a direct push to `main` is refused: `GH013: Repository rule violations ... Changes must be made through a pull
  request. 5 of 5 required status checks are expected.`, `main` unchanged;
- a pull request whose tests fail (#8: `result` failure, no `AI review` status) cannot be merged;
- a push after the AI review (#6: the reviewed head had `AI review` = success; after a new commit the new head has no
  `AI review` status until the review runs again) cannot be merged;
- force push: the `non_fast_forward` rule is active (read from the API); not attempted, because if the rule failed it
  would rewrite the history of `main`.

Not checked here: that a person's approval is dismissed by a new push. GitHub implements it
(`dismiss_stale_reviews_on_push: true`), but in a one-person sandbox nobody can approve: GitHub does not let the author
approve their own pull request. To check it: a second account with write access approves a pull request, the author
pushes a commit, the approval shows as dismissed and the merge is blocked again.
