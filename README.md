# github-action

[![Tests](https://github.com/a11ywatch/github-actions/actions/workflows/action.yml/badge.svg)](https://github.com/a11ywatch/github-actions/actions/workflows/action.yml)

A feature rich GitHub action that runs actionable accessibility reports on your website that can handle large workloads.

Some of the primary features include pass/fail testing, code fixes, and detailed reports.

When running locally the action uses A11yWatch Lite.

Publish comment in every event

### Usage

```yaml
- uses: neildan/web_accessibility_evaluation@v0.5
  with:
    WEBSITE_URL: ${{ secrets.WEBSITE_URL }}
    SUBDOMAINS: true
    LIST: true
    FIX: true
    TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

### Action inputs

All inputs are **optional** except $WEBSITE_URL.

| Name                  | Description                                                                                                                                                                                                              | Default        |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------------- | --- | ----- |
| `WEBSITE_URL`         | Website domain to scan (Start with http:// or https://).                                                                                                                                                                 |                |
| `SITE_WIDE`           | Site-wide scanning across all pages.                                                                                                                                                                                     | false          |
| `FIX`                 | Attempt to apply recommendations to code and commit to github.                                                                                                                                                           | false          |
| `SUBDOMAINS`          | Include all subdomains (required SITE_WIDE=true).                                                                                                                                                                        | true           |
| `SITEMAP`             | Extend crawl with sitemap links (required SITE_WIDE=true).                                                                                                                                                               | true           |
| `TLD`                 | Include all tld extensions (required SITE_WIDE=true).                                                                                                                                                                    | true           |
| `LIST`                | Report the results to github as a pass or fail list or detailed report.                                                                                                                                                  | false          |
| `FAIL_TOTAL_COUNT`    | Determine whether to fail the CI if total issues warnings and errors exceed the counter. Takes precedence over the other FAIL inputs.                                                                                    | 0              |
| `FAIL_ERRORS_COUNT`   | Determine whether to fail the CI if total issues with errors exceed the counter.                                                                                                                                         | 0              |
| `FAIL_WARNINGS_COUNT` | Determine whether to fail the CI if total issues with warnings exceed the counter.                                                                                                                                       | 0              |
| `EXTERNAL`            | Use the A11yWatch remote API for fast results. If this is set `A11YWATCH_TOKEN` is needed.                                                                                                                               |                |     | false |
| `DISABLE_PR_STATS`    | Prevent messaging to the pr results of test.                                                                                                                                                                             | false          |
| `TOKEN`               | `GITHUB_TOKEN` (permissions `contents: write` and `pull-requests: write`) or a `repo` scoped [Personal Access Token (PAT)](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token). | `GITHUB_TOKEN` |
| `A11YWATCH_TOKEN`     | The A11yWatch API token to use to identify a user.                                                                                                                                                                       |                |
| `UPGRADE`             | Upgrade the docker images before testing to latest.                                                                                                                                                                      |                |

### Action Outputs

| Name                 | Description                | Default |
| -------------------- | -------------------------- | ------- |
| `issues`             | The amount of issues found |         |
| `result_json_issues` | The issues descriptions    |         |

## Benches

The hardware specs for results:

```sh
----------------------
linux ubuntu-latest
2-core CPU
7 GB of RAM memory
14 GB of SSD disk space
-----------------------
```

### Benchmark Results

The results for crawling a website that is small - large.

#### crawl-speed

Test url: `https://a11ywatch.com`

25 pages

runs with 10 samples:

|                        | `libraries`            |
| :--------------------- | :--------------------- |
| **`A11yWatch: crawl`** | `0.4 s` (✅ **1.00x**) |
| **`Pa11y-CI: crawl`**  | `45 s` (✅ **1.00x**)  |
| **`Axe: crawl`**       | `N/A` (✅ **1.00x**)   |

#### crawl-speed (Large Website)

Test url: `https://www.hbo.com`

7500 pages.

runs with 10 samples:

|                        | `libraries`               |
| :--------------------- | :------------------------ |
| **`A11yWatch: crawl`** | `2.5 mins` (✅ **1.00x**) |
| **`Pa11y-CI: crawl`**  | `50+ hr` (✅ **1.00x**)   |
| **`Axe: crawl`**       | `N/A` (✅ **1.00x**)      |

### Benchmark Info

On a larger website A11yWatch action runs over 60x-10,000x+ faster depending on CPUs/hardware.

When `AI_DISABLED` is set to true the run for `A11yWatch` may increase.

## Common Issues

If you experience issues on your CI you may have to toggle the `UPGRADE` input to true in order to get the latest docker images.
We need docker in order to build the appliciation in quickly since we have some services that need to compile that may take awhile.

## CLI

In order to get the results of the run in json run `results_json="$(a11ywatch -r)"` across any step after the action.

## LICENSE

check the license file in the root of the project.
