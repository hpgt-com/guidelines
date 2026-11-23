# Security

## Reporting a security issue

Do not open a public issue. Use **Report a vulnerability** under this repository's **Security** tab, so only the
maintainers see the report. Describe what you found, how to reproduce it and what it affects. We reply as soon as we
can, but this is a volunteer club project.

## What is in place

- All changes to `main` go through a pull request (branch ruleset «Main protection»). Force push and deletion of `main`
  are blocked.
- Required checks: `dependency-review` and `workflow-security` (zizmor), plus CodeQL code scanning of the workflows
  (high or higher blocks the merge).
- Actions are pinned to commit SHAs and updated monthly by Dependabot, with a cooldown of 7 days (30 for major
  versions). Patch and minor updates of Actions are merged automatically when all required checks pass
  (`dependabot-automerge.yml`). Other updates are merged by hand.
- Workflows get read-only access by default, with `persist-credentials: false` on every checkout.
- The repository holds Markdown only. It has no secrets, environments or deployments.
