# Instructions for Changes in This Fork

This repository tracks the upstream `shadcn-ui/ui` monorepo. Read `FORK_MAINTENANCE.md` and `CONTRIBUTING.md` before changing source.

- Treat upstream source, package names, licences, release workflows and registry contracts as vendor-owned.
- Do not implement project-specific styling, authentication, business data or BIM/Revit integrations here. Those belong in `ryanwhois/interface-system` or the consuming application.
- Keep proposed upstream fixes isolated and small, and document the upstream base SHA and compatibility impact.
- Do not automatically merge/rebase upstream, force-push branches, publish npm packages or enable credential-using workflows as part of a routine upgrade.
- For an upstream sync, inspect changed scripts and dependencies, then run the applicable pnpm checks before opening a PR.
- Preserve MIT notices and do not copy paid Tailwind/Catalyst source into this fork.
- Consumers install generic components through the shadcn CLI; `interface-system` is the separately published shared component package.
