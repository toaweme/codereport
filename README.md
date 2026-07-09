# codereport

[![GitHub Tag](https://img.shields.io/github/v/tag/toaweme/codereport?label=Tag&color=green)](https://github.com/toaweme/codereport/releases)
[![License](https://img.shields.io/badge/License-Apache_2.0-blue)](/LICENSE)

## Code reports for code.toawe.me

`codereport` reads a repo's Go source into a `code.json` code view and publishes
it to the [code.toawe.me](https://code.toawe.me) dashboard, so it renders that
repo's packages, types, and stats next to its `care` health report.

This repo holds **no source**, only the GitHub Action below and the released
binaries.

## Usage

Add the action to a repo's CI. It installs a verified `codereport` release,
generates the code view, and POSTs it to the dashboard. No secrets: the OIDC
token is minted at runtime and proves which repo the report came from.

```yaml
permissions:
  id-token: write        # mint the OIDC token; the only permission required
  contents: read

steps:
  - uses: actions/checkout@v4
  - uses: toaweme/codereport@v1
    with:
      publish-url: https://code.toawe.me/ingest?kind=code
```

The report is produced by type-checking the repo, so the job must be able to
resolve the repo's own Go dependencies.

### Inputs

| Input | Default | Description |
| --- | --- | --- |
| `publish-url` | `""` | Endpoint to POST the report to. Omit to only generate. |
| `dir` | `.` | Directory to read, for a module in a subdirectory with its own `go.mod`. |
| `output` | `code.json` | Where the report is written in the workspace. |
| `version` | pinned per release | Release tag to install. Usually leave unset. |
| `verify` | `true` | Verify the cosign signature on `checksums.txt`. |
| `install-only` | `false` | Install `codereport` on PATH and stop. |
| `publish-timeout` | `30` | Max seconds per publish call. |

### Output

| Output | Description |
| --- | --- |
| `report-path` | Path to the generated `code.json` in the workspace, empty if none was produced. |

---

Made with ❤️ in Lithuania 🇱🇹.
