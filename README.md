# vulnerable-list

A **demo-only** repository of manifest files that pin known-vulnerable versions of open source packages. Used as a controlled Sourcegraph search target for supply-chain attack response walkthroughs.

## ⚠️ Do not install, build, or run

Every file in `incidents/` is **inert text**:

- `package.json` files are not consumed by `npm install` — nothing in this repo is run, packaged, or shipped.
- `requirements.txt`, `Gemfile`, `go.mod`, `pom.xml`, and workflow YAML files are committed for indexing only.
- There is no CI, no Dependabot, no GitHub Actions workflow that processes the manifests. All security analysis features should be disabled on this repo.

The pinned versions are real vulnerable versions disclosed via the GitHub Advisory DB. Their presence here lets Sourcegraph index a guaranteed hit for the demo, so detection queries always return results during a video walkthrough — even when no live customer codebase happens to pin the affected version.

## Layout

```
incidents/
  <package-slug>-<YYYY-MM-DD>/
    <manifest-file>            e.g. package.json, requirements.txt, Gemfile, go.mod, pom.xml, .github/workflows/demo.yml
```

Each subdirectory corresponds to one disclosed advisory. The slug matches the `id` field of the incident descriptor used by the supply-chain response automation.

## How entries are added

The [supply-chain-playbook](https://github.com/sourcegraph/supply-chain-playbook) automation auto-PRs a manifest here when a new GitHub Advisory is triaged. Each PR is auto-merged via the playbook's seed step. See `automation/seed-demo-repo.js` in that repo for the source.

## Removing an entry

When a vulnerable version is delisted upstream or no longer useful for demos, delete the matching `incidents/<id>/` directory and commit. Sourcegraph will re-index on next crawl.

## Settings checklist (one-time)

- Dependabot security & version updates: **disabled**
- GitHub Actions: **disabled** (no workflows should run)
- Branch protection on default branch: optional, but if enabled, the seed token's PAT must be allowed to merge PRs
