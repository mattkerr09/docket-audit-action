# Docket SEO audit for GitHub Actions

Runs a Docket audit of a site in your workflow and fails the job when it finds a problem at or above the severity you set. Use it after a deploy to catch a noindex on the homepage, a broken canonical or a site that stopped answering.

It runs Docket's free version, so it needs no licence key and no account. The free version gives the score, the count of problems in each area and the most important problem in full, and it exits with code 2 when the run reaches your `fail-on` level. That is enough to gate a deploy.

## Example

```yaml
name: SEO check
on:
  push:
    branches: [main]

jobs:
  docket:
    runs-on: ubuntu-latest
    steps:
      - uses: mattkerr09/docket-audit-action@v1
        with:
          url: https://staging.example.com
          fail-on: critical
```

The audit text goes in the job log and in the job summary.

## Inputs

| Input | Default | What it does |
|---|---|---|
| `url` | (required) | The site to audit. |
| `fail-on` | `critical` | Fail at this severity or worse: critical, high, medium, low, notice or never. |
| `pages` | `100` | How many pages to crawl. |
| `version` | `latest` | A Docket release tag, such as `v1.3.147`. |

The `exit-code` output is 0 when the run passed, 2 when it found problems at or above `fail-on`, and 1 when Docket could not run at all.

## Runners

Linux x86_64 (`ubuntu-latest`) and macOS on Apple silicon or Intel. There is no Linux ARM or Windows build of the command line in this action yet.

## What it does not do

The free version does not list every problem or write SARIF or JUnit files. Those come with a licence, which activates on up to 3 computers at a time. A fresh CI runner counts as a new computer each time, so this action sticks to the free version.

Docket and the full app: https://docketseo.app
