# updatebot

Central dependency-update policy for the `MarcoPeretti/*` sites (mailsrv, ll, unspam, knull, randompunctuation.com).

- `default.json`: shared Renovate preset. Repos extend it with `{ "extends": ["github>MarcoPeretti/updatebot"] }`.
  - 5-day minimum release age for all updates, including security fixes. Security PRs open immediately, labelled `security`, and automerge once the fix is 5 days old and CI is green.
  - Patch/minor/digest automerge. Majors batch monthly and never automerge. Terraform, Dockerfiles: PR only.
- `.github/workflows/ci-node.yml`, `ci-go.yml`: reusable CI for repos without their own.
- `.github/workflows/security-scan.yml`: reusable `govulncheck` + `osv-scanner`; call it from a weekly `schedule:` workflow in each repo.
- `.github/workflows/quarterly-reminders.yml`: quarterly checklist issue for what Renovate can't see (wrangler `compatibility_date`, Lambda runtimes, unapplied Terraform, undeployed merges).

Action versions in these workflows are tags; Renovate pins them to digests once this repo has the app installed.

Calling a reusable workflow from another private repo requires this repo's Settings > Actions > Access to allow access from repositories of the same owner.
