# renovate-presets

Shareable [Renovate](https://docs.renovatebot.com) custom managers and rules I recreate across several projects.

## Usage

### Extend everything

```jsonc
{
    "$schema": "https://docs.renovatebot.com/renovate-schema.json",
    "extends": ["github>sironheart/renovate-preset"],
}
```

The bare repo reference loads [`default.json`](./default.json), which extends every preset in this repo.

### Extend specific presets

Use Renovate's path-based preset syntax (include the `.json5` extension since Renovate's default resolver only tries `.json`):

```jsonc
{
    "extends": ["github>sironheart/renovate-preset//versioning/biomeGrouping.json5"],
}
```

### Pin to a tag or commit

```jsonc
"extends": ["github>sironheart/renovate-preset#1.0.0"]
```

## Available presets

Presets are grouped by intent. Each file is self-documenting — open it for the `description` and any caveats.

- [`config/`](./config) — top-level Renovate config (registry aliases, built-in manager file-pattern extensions).
- [`language/`](./language) — rules for language-specific tooling and ecosystems.

<!-- - [`overrides/`](./overrides) — fixes to `depName`, `sourceUrl`, `packageName`, or `changelogUrl` for specific packages or managers. -->

- [`policies/`](./policies) — opt-in `packageRules` that shape commit messages. Extend only if you want the convention.
    - [`policies/githubActionsGrouping.json5`](./policies/githubActionsGrouping.json5) — group GitHub-hosted actions by owner/repository, pairing short GitHub references with explicit `https://github.com/` references in Forgejo workflows. Included in the default preset.
    - Automerge update creation windows do not overlap: lock maintenance Monday 00:00–04:00, Renovate/formatters 04:00–08:00 every second day, mise daily 08:00–12:00. Times use the configured Renovate timezone (UTC by default). Platform automerge remains enabled, so merging can happen outside these windows.
    - [`policies/semanticCommitMessagesCi.json5`](./policies/semanticCommitMessagesCi.json5) — always use non-breaking `ci` commits for [CI managers](https://docs.renovatebot.com/configuration-options/#packagerulesmatchcategories), including GitHub/Forgejo/Gitea Actions and GitLab CI. Included in the default preset; when extending individually, load it after other commit-message policies. Major updates omit `!`, but can still break your pipelines.
- [`versioning/`](./versioning) — custom `versioning` schemes for upstreams that ship non-semver tags.

[`default.json`](./default.json) lists every preset that ships in this repo.
