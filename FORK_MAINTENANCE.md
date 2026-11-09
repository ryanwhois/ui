# Maintaining the shadcn/ui Fork

**Repository role:** An upstream-tracking source and CLI reference for `shadcn-ui/ui`. The product-level design system belongs to [`ryanwhois/interface-system`](https://github.com/ryanwhois/interface-system).

This repository is a real GitHub fork of `shadcn-ui/ui`. At the 8 November 2026 audit, `main` had no identified local commits ahead of the upstream branch and was **87 upstream commits behind**. This is a *comparison at that time*, not a continuous monitor. The upstream README, `pnpm` monorepo, registry, package CLI and shadcn release workflows are preserved. Fork-level upstream release/monitor jobs are conditioned on `github.repository_owner == 'shadcn-ui'` or `github.repository == 'shadcn-ui/ui'`; they are **not running as your own release/sync automation**.

## Responsibility Boundaries

| Concern | Canonical owner |
| --- | --- |
| Upstream shadcn CLI, primitives, registry tooling and upstream source fixes | `shadcn-ui/ui` / this tracking fork for integration work |
| Ryan's reusable accessible primitives, validated design tokens, composed controls and workspace patterns | `ryanwhois/interface-system` |
| Domain-specific rules, geometry, tool actions, layouts and adapters | The consuming project (for example `ryanwhois/auto-revit`) |
| Reusable project-neutral UI extracted from at least one proven use case | Proposed PR to `interface-system`, after its component contract is made generic |

**Do not copy a modified upstream component back into this fork merely to make one project look different.** Prefer semantic tokens or a composition in Interface System. Only patch upstream code here to fix a general upstream defect, to experiment with a CLI/registry change, or to prepare an upstream contribution. Keep each patch small and traceable.

This fork does not publish `@ryanwhois/interface-system` and should not publish shadcn's upstream npm packages under their original names.

## Safe Upstream Sync

From a clean checkout of this fork:

```bash
git remote -v
git remote add upstream https://github.com/shadcn-ui/ui.git  # only if missing
git fetch origin --prune
git fetch upstream main
git switch main
git pull --ff-only origin main
git rev-list --left-right --count main...upstream/main
```

That last command prints `<fork-ahead> <upstream-ahead>`. Check both numbers before integrating.

When `main` is strictly behind upstream (fork-ahead = 0), use a temporary integration branch and fast-forward it to upstream; **do not force-push or reset main**:

```bash
git switch -c chore/sync-shadcn-upstream
git merge --ff-only upstream/main
# inspect new dependencies and workflows; run the checks below
git push -u origin chore/sync-shadcn-upstream
# open a PR targeting main
```

If the fork also has its own commits, `--ff-only` will refuse. **Stop and review the divergence**; integrate upstream into a separate PR, resolve conflicts carefully, and preserve both histories. Do not blindly overwrite custom changes or auto-merge into main.

### Checks before merging a sync PR

- Review the upstream commit range and especially `package.json`, `pnpm-lock.yaml`, `packages/shadcn`, `apps/v4/registry`, third-party notices, workflow permissions and scripts.
- Use the pinned toolchain in `package.json` and `pnpm-workspace.yaml` (currently pnpm 10.33.4). The current upstream monorepo uses Node 24 in its release workflow; check package engine constraints before choosing Node.
- Run `pnpm install --frozen-lockfile`, `pnpm check`, and the relevant package tests and builds. Follow `CONTRIBUTING.md` for registry-specific checks. An upstream upgrade may change these commands; confirm from the synced repository.
- Ensure no unintended npm publish, registry deployment, secret access or personal branding changes are inherited into fork workflows.
- Record the new upstream SHA, sync date, changed interfaces, migration work and consumer compatibility notes in the PR. Never assume the fork's lack of Actions runs is a green validation result.

## Keep Interface System Current Without Locking It to the Fork

Inside `interface-system`, inspect a component before adopting new source:

```bash
npx shadcn@latest add tooltip --dry-run --diff
```

Review the generated diff, upstream licence and exact Radix/Base UI base. Do not automatically overwrite locally adapted components. Create a focused interface-system PR with an example, accessibility tests, package-consumer verification and a changelog entry.

The fork can track upstream *independently* of Interface System releases; updating one does not automatically ship updates to other projects.

## Review Cadence

Check upstream divergence monthly and before a framework or registry upgrade. Record commit counts and any breaking changes in a tracked issue or PR. Avoid an unattended job that pushes upstream directly into production branches.

## Related Guidance

- [Interface System architecture](https://github.com/ryanwhois/interface-system/blob/main/docs/architecture.md)
- [Interface System primitive adoption](https://github.com/ryanwhois/interface-system/blob/main/docs/adopting-shadcn.md)
- [Official shadcn CLI](https://ui.shadcn.com/docs/cli)
- [Official registry namespaces](https://ui.shadcn.com/docs/registry/namespace)
