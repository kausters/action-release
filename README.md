# action-release

Composite GitHub Actions for release automation.

## Actions

### `kausters/action-release@v1` — Full release flow

Creates or updates a draft GitHub release with auto-generated release notes. Calculates the next version from the latest release and `package.json`.

```yaml
- uses: kausters/action-release@v1
```

No inputs required. Needs `contents: write` permission.

---

### `kausters/action-release/get-next-version@v1` — Version calculation

Calculates the next version based on the latest published release and `package.json`. Bumps patch if the latest release major/minor matches `package.json`; otherwise uses `package.json` version as-is.

```yaml
- id: version
  uses: kausters/action-release/get-next-version@v1

- run: echo "Next version is ${{ steps.version.outputs.version }}"
```

**Outputs**

| Output | Description |
|--------|-------------|
| `version` | The calculated next version (without `v` prefix) |
| `latest` | The latest published release tag |

---

### `kausters/action-release/float-major-tag@v1` — Float major tag on publish

Moves (or creates) a floating major version tag (e.g. `v1`) to point at a newly published release. Intended to run on `release: published`.

```yaml
on:
  release:
    types: [published]

jobs:
  tag:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - uses: kausters/action-release/float-major-tag@v1
```

No inputs required. Reads `github.event.release.tag_name` and `github.sha` automatically.
