# LifeForge Registry

Static index of installable **modules** and **language packs**, consumed by the ForgeCLI.

The CLI resolves installs/upgrades from these files and verifies each download against the
pinned commit — there is no server.

## Files

- `modules.json` — module index
- `locales.json` — language-pack index

## Entry schema

```jsonc
{
  "id": "lifeforge--wallet",                 // scope-stripped package id
  "repo": "Lifeforge-app/lifeforge-module-wallet",
  "latest": "0.0.6",                          // latest published semver
  "versions": {
    "0.0.5": "<commit sha>",
    "0.0.6": "<commit sha>"                   // immutable release commit
  }
}
```

## How it is updated

`forge modules publish` / `forge locales publish`:

1. validate the package and its author,
2. bump the version, commit and push a `v<version>` git tag,
3. add `versions[<version>] = <tag commit sha>` (and update `latest`).

Community publishers open a PR; maintainers commit directly.

## Bootstrapping / rebuilding

`scripts/seed-registry.mjs` (in the main repo) regenerates both files from the GitHub
repos, using each repo's `package.json` for the id/version and the default-branch HEAD as
the pinned commit (until tags exist).
