# Protecting the default branch

The checks only colour a pull request; without branch protection anyone with write access can merge a red one
(seen in the sandbox on 2026-10-27: an edit to `test.yaml` and a module that failed its tests were merged). The
rules below make the checks a gate. Set them as a ruleset (Settings → Rules → Rulesets → New branch ruleset) on the
default branch.

| Setting | Value | Why |
|---|---|---|
| Enforcement | Active | |
| Bypass list | **empty** | owners and admins go through the same gate (LIC-614: "the bypass rights are accounted for"); a bypass let a pull request into `main` without its checks in the sandbox, see below |
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
image update's own run (linked from the pull request). Ways out, MIKO's choice:

- **B. A GitHub App for the image update** (recommended): the workflow opens the pull request with the App's token
  (stored as a secret in its own environment, `image-update`), so the normal checks run on it and no bypass is
  needed. One more credential to own.
- **Manual**: an admin sets the ruleset to *Disabled*, merges the image update pull request and sets it back to
  *Active*. GitHub records both changes in the audit log. Use rarely; nothing stops another merge in that window.
- **A. Admin bypass for pull requests only** — **not recommended.** With *Repository admin* in the bypass list
  (mode *For pull requests only*) direct pushes stay blocked, but an admin can merge **any** pull request without
  its checks, and so can an agent acting through a GitHub App on that admin's behalf. The convention "only image
  update pull requests" is not enforced and was broken in the sandbox (below).

Also: Settings → Actions → General → "Allow GitHub Actions to create and approve pull requests" on (needed for the
workflow to open its pull request at all).

## In the sandbox (2026-10-27)

Ruleset `main: applications gate` (id 25947319) on excla1mmm/module-applications follows the table with an **empty
bypass list**, and differences for a one-person sandbox: **0 required approvals** (since 2026-10-27, M04: GitHub does
not let the only account approve its own pull request, so with 1 nothing could be merged; the real repository keeps 1 or
more), "Require approval of the most recent reviewable push" is **off** and code owner review is off (no CODEOWNERS
yet). The five required checks stay required. The checks below were made with 1 approval required.

**What went wrong first.** The ruleset was created at 09:52Z with option A (repository admin, pull requests only).
At 10:05Z pull request #6 was merged into `main` through that bypass, by hand, while its head 90adbc2 had no `AI review`
status and `agreement` failed. Nothing in GitHub limits option A to image update pull requests, and an agent acting
through a GitHub App on an admin's behalf has the same power. The bypass was removed, and #6 was reverted by #10
(merged with the ruleset briefly set to *Disabled*; both changes are in the audit log). The agents never merged #6.

Checked again after that, with no bypass (ruleset read through the API: `enforcement: active`, `bypass_actors: []`,
`current_user_can_bypass: never`):

- a direct push to `main` is refused: `GH013: Repository rule violations ... Changes must be made through a pull
  request. 5 of 5 required status checks are expected.`, `main` unchanged (42d9f17);
- a pull request whose tests fail (#8: `result` failure, no `AI review` status) — a merge through the API as the
  repository admin is refused: `405 Repository rule violations found. At least 1 approving review is required ...
  5 of 5 required status checks are expected.`;
- a pull request with AI review blockers (#7: `AI review` failure) — the same 405;
- a push after the AI review (#11, a maintenance probe: the first head 2bdd585 got `AI review` = success; the next
  commit 1a975f4 had no statuses at all) — a merge through the API right after the push is refused: `405 ... 5 of 5
  required status checks have not succeeded: 2 expected.` The probe was closed unmerged;
- force push: the `non_fast_forward` rule is active (read from the API); not attempted, because if the rule failed it
  would rewrite the history of `main`.

Not checked here: that a person's approval is dismissed by a new push. GitHub implements it
(`dismiss_stale_reviews_on_push: true`), but in a one-person sandbox nobody can approve: GitHub does not let the author
approve their own pull request. To check it: a second account with write access approves a pull request, the author
pushes a commit, the approval shows as dismissed and the merge is blocked again.
