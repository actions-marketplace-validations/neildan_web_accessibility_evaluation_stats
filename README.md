# github-action

[![Tests](https://github.com/neildan/web_accessibility_evaluation_stats/actions/workflows/action.yml/badge.svg)](https://github.com/neildan/web_accessibility_evaluation_stats/actions/workflows/action.yml)

A feature rich GitHub action that runs actionable accessibility reports on your website that can handle large workloads.

Some of the primary features include pass/fail testing, code fixes, and detailed reports.

When running locally the action uses A11yWatch Lite.

Get complete information about web accessibility testing, this tool only returns the evaluation, no actions involved

### Usage

```yaml
- uses: neildan/web_accessibility_evaluation_stats@v1
  with:
    WEBSITE_URL: ${{ secrets.WEBSITE_URL }}
    SUBDOMAINS: true
    LIST: true
    SITEMAP: true
    FAIL_TOTAL_COUNT: 1
```

### Action inputs

All inputs are **optional** except $WEBSITE_URL.

| Name                  | Description                                                                                                                              | Default |
| --------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| `WEBSITE_URL`         | Website domain to scan (Start with http:// or https://).                                                                                 |         |
| `SITE_WIDE`           | Site-wide scanning across all pages.                                                                                                     | false   |
| `FIX`                 | Attempt to apply recommendations to code and commit to github.                                                                           | false   |
| `SUBDOMAINS`          | Include all subdomains (required SITE_WIDE=true).                                                                                        | false   |
| `SITEMAP`             | Extend crawl with sitemap links (required SITE_WIDE=true).                                                                               | false   |
| `TLD`                 | Include all tld extensions (required SITE_WIDE=true).                                                                                    | false   |
| `LIST`                | Report the results to github as a pass or fail list or detailed report.                                                                  | false   |
| `FAIL_TOTAL_COUNT`    | Determine whether to fail the CI if total issues warnings and errors exceed the counter. Takes precedence over the other FAIL inputs.    | 0       |
| `FAIL_ERRORS_COUNT`   | Determine whether to fail the CI if total issues with errors exceed the counter.                                                         | 0       |
| `FAIL_WARNINGS_COUNT` | Determine whether to fail the CI if total issues with warnings exceed the counter.                                                       | 0       |
| `SLIM`                | Use the gRPC client to gather reports - only displays stats, useful for large websites (no code generation, no outputs, just pure stats) | true    |
| `UPGRADE`             | Upgrade the docker images before testing to latest.                                                                                      | false   |

### Action Outputs

| Name                  | Description                                            | Default |
| --------------------- | ------------------------------------------------------ | ------- |
| `AMOUNT_ISSUES_TOTAL` | The amount of errors and warnings found on the page    | 0       |
| `AMOUNT_ERRORS`       | The amount of errors found on the page                 | 0       |
| `AMOUNT_WARNINGS`     | The amount of warnings found on the page               | 0       |
| `EVALUATION_STATUS`   | The status of end evaluation, show 'success' or 'fail' | 'fail'  |
| `JSON_FULL_RESULT`    | A full message with evaluation description             | ''      |
| `FULL_EVALUATION`     | The issues descriptions found on the page              | ''      |

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
