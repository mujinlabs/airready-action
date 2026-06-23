# airready-action

GitHub Action for [**airready**](https://github.com/mujinlabs/airready) — grade how
ready a site is for AI agents & LLM crawlers (llms.txt, AI-bot policy, structured data,
server-rendered content, sitemap…) and **fail CI when it regresses below your bar**.

Point it at your production or preview URL on a schedule or before deploy, so an
AI-readiness drop (someone ships a JS-only shell, deletes `llms.txt`, breaks JSON-LD)
gets caught like any other check.

## Usage

```yaml
# .github/workflows/ai-readiness.yml
name: ai-readiness
on:
  schedule: [{ cron: "0 7 * * 1" }]   # weekly
  workflow_dispatch:

permissions:
  contents: read

jobs:
  grade:
    runs-on: ubuntu-latest
    steps:
      - uses: mujinlabs/airready-action@v1
        with:
          url: https://yoursite.com
          min-grade: B          # fail if the site drops below a B
          # min-score: 75        # or gate on the exact score instead
```

The job fails (exit 1) when the live site scores below `min-score` or grades below
`min-grade`. Set neither to just print the report without gating.

## Inputs

| Input | Default | Description |
|---|---|---|
| `url` | _(required)_ | URL to grade (usually your production or preview site) |
| `min-score` | _(off)_ | Fail if the AI-readiness score is below this (0–100) |
| `min-grade` | _(off)_ | Fail if the grade is below this (`A` best, `F` worst) |
| `version` | pinned | `mujin-airready` version to install |

Want the human-facing report and an embeddable badge instead of a CI gate? Use the
hosted grader at **[airready.mujinlabs.com](https://airready.mujinlabs.com)**.

MIT · built by [Mujin Labs](https://github.com/mujinlabs).
