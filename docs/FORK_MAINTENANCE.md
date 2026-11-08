# Fork Maintenance — ryanwhois/ui

## Purpose

This repository is a **close-to-upstream shadcn/ui fork**. It is not the default installation source for consumer applications and does not replace the `@ryanwhois/interface-system` package.

- **Upstream** `shadcn-ui/ui`: source of generic primitive implementations, shadcn CLI, registry format, documentation, upstream fixes and security patches.
- **Fork** `ryanwhois/ui`: deliberate experiments and optional upstream contributions. Keep upstream mergeability and original licences/notices intact.
- **Interface System** `ryanwhois/interface-system`: reviewed application-facing React 19 package, themes, composed controls, docs, tests and stable versions.
- **Consumers** (AutoRevit and others): install/pin Interface System; app-specific workflow and model integrations remain in the application.

Do **not** maintain separate customized copies of `Button`, `Dialog`, `Table`, etc. in this fork solely to change branding. Prefer semantic CSS tokens in Interface System. Upstream files should be changed here only when the edit is a generic fix suitable for eventual contribution to shadcn.

## Audit Snapshot — 8 November 2026

- This is a GitHub fork of `shadcn-ui/ui`, default branch `main`.
- Observed fork head: `2d3f1cd436b18ea12f24130de4df781355925b08`.
- Observed upstream head: `8ab27715521cbd4f87ae6886cd989cbb678f6de7`.
- GitHub compare reported this fork **87 upstream commits behind, zero ahead**. This is a point-in-time result, not a continuously verified guarantee.
- Root `README.md` and `CONTRIBUTING.md` were originally inherited upstream content.
- Repository is a pnpm/Turbo/Changesets monorepo with many upstream-specific workflows. In particular `monitor-registries.yml` and `release.yml` have conditions restricting important actions to the upstream owner. No evidence of a separately configured fork release pipeline was established.
- Do not infer that Actions is green: no recent workflow runs were returned by the repository audit.

## Updating Without Losing Work

Use a **reviewable** update branch. Never force-sync or reset `main` just to catch up. On a workstation:

```bash
git clone https://github.com/ryanwhois/ui.git
cd ui
git remote add upstream https://github.com/shadcn-ui/ui.git
git fetch origin main
git fetch upstream main
git checkout -b maintenance/sync-shadcn-YYYYMMDD origin/main
git merge --no-edit upstream/main
# Resolve any conflicts, test, commit, then push the sync branch and open a PR.
```

If Git reports conflicts, resolve them instead of using `gh repo sync --force`. For a change to source files:
- preserve the upstream implementation by default;
- isolate customisations in separate files or Interface System compositions;
- inspect package/registry schema migrations, Node/pnpm changes and lockfile churn;
- retain third-party attribution and licences;
- record the exact upstream SHA merged and any deferred conflicts in the PR.

The `gh repo sync ryanwhois/ui -b main` shortcut can work for a clean fast-forward, but a **PR** is preferred because this fork may evolve local changes and the upstream update can be large.

## Maintenance Cadence

- **Weekly:** inspect compare status and open security fixes; use [GitHub compare](https://github.com/ryanwhois/ui/compare/main...shadcn-ui:main) or `git rev-list --left-right --count origin/main...upstream/main`.
- **Before a consumer release:** check upstream changes affecting adopted primitives and test selected upgrades in `interface-system` first.
- **On a security advisory:** prioritise exposure analysis, upgrade/sync and downstream release, not waiting for the next scheduled review.
- **On a major registry or Radix/Base UI change:** do a compatibility spike rather than mixing two competing primitive bases in one application.

Shadcn currently supports namespaced registries, including third-party/private sources. This does **not** require publishing your full shadcn fork or changing its CLI. See https://ui.shadcn.com/docs/registry/namespace .

## Sync Acceptance Checklist

1. Record upstream and fork SHAs and ahead/behind status in the update PR.
2. Verify Node/pnpm versions against this repository's `package.json` and `pnpm-workspace.yaml`.
3. Run `pnpm install --frozen-lockfile`.
4. Run targeted registry build and checks, then the relevant complete build/lint/typecheck/test commands supported by the imported revision.
5. Verify generated registry indexes and provenance; avoid committing ignored generated output.
6. Run application-level consumer tests on any primitive changes adopted into `interface-system`.
7. Merge reviewed changes into fork `main` without force-push.

The fork is an upstream source mirror with curated experiments—not a second published design system. **Passing this fork's checks does not imply downstream Interface System compatibility.**

## Contribution Back to the Shared UI System

When AutoRevit or another project needs a UI component:
1. Search Interface System's exports and playground catalogue.
2. Check the shadcn primitive upstream first.
3. Keep domain-specific business logic inside the project.
4. Propose a generalised component **in Interface System**, with typed props/callbacks, accessibility/test cases, a playground example and a consumer-package test.
5. Use the fork only for upstream-compatible primitive fixes or registry experiments. Where appropriate, contribute generic fixes upstream.
6. Release a new Interface System version after CI and adoption review; consumers pin that version and upgrade deliberately.

See https://github.com/ryanwhois/interface-system/blob/main/docs/architecture.md and its adoption instructions.
