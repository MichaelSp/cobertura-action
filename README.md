# Cobertura action

![](https://github.com/5monkeys/cobertura-action/workflows/Test/badge.svg)

GitHub Action which parse a [XML cobertura report](http://cobertura.github.io/cobertura/) and display the metrics in a GitHub Pull Request.

Many coverage tools can be configured to output cobertura reports:

* [coverage.py](https://coverage.readthedocs.io/en/latest/cmd.html#xml-reporting)
* [Istanbul](https://istanbul.js.org/docs/advanced/alternative-reporters/#cobertura)
* [Maven](https://www.mojohaus.org/cobertura-maven-plugin/)
* [simplecov](https://github.com/colszowka/simplecov/blob/master/doc/alternate-formatters.md#simplecov-cobertura)

This action will not currently work when triggered by pull requests from forks, like is common in open source projects.
This is because the token for forked pull request workflows does not have write permissions on the target repository.
Hopefully GitHub will have a solution for this in the future. In the meantime one can use utilize multiple workflows and
artifacts to circumvent this. See the workflows in this project for an implementation example and this blog post https://securitylab.github.com/research/github-actions-preventing-pwn-requests.


## How it looks like

![alt text](img/comment.png "Pull request comment with metrics")

## Inputs

| Input                   | Description                                                                                                                        | Default         | Required |
|-------------------------|------------------------------------------------------------------------------------------------------------------------------------|-----------------|----------|
| `repo_token`            | The GITHUB_TOKEN for this repo. See [details](https://help.github.com/en/articles/virtual-environments-for-github-actions#github_token-secret). e.g: `${{ secrets.GITHUB_TOKEN }}`   |                 | :check:  |
| `path`                    | Path to the cobertura file. Glob pattern is supported.                                                                             | `**/coverage.xml` |          |
| `skip_covered`            | If files with 100% should be skipped from report.                                                                                  | `true`            |          |
| `minimum_coverage`        | Minimum allowed coverage percentage as an integer.                                                                                 | `60`              |          |
| `show_line`               | Show line rate as specific column.                                                                                                 | `false`           |          |
| `show_branch`             | Show branch rate as specific column.                                                                                               | `true`            |          |
| `show_class_names`        | Show class names instead of file names.                                                                                            | `false`           |          |
| `show_missing`            | Show line numbers of statements, per module, that was not executed.                                                                | `false`           |          |
| `show_missing_max_length` | Crop missing line numbers strings that exceeds this length, provided as an integer. (Note: "..." is appended to a cropped string)  | `''`              |          |
| `only_changed_files`      | Only show coverage for changed files.                                                                                              | `false`           |          |
| `pull_request_number`     | Pull request number associated with the report. This property should be used when workflow trigger is different than pull_request. | `''`              |          |
| `report_name`             |  Use a unique name for the report and comment. | `''` | |


## Example usage

```yaml
on:
  pull_request:
    types: [opened]
    branches:
      - master
jobs:
  coverage:
    runs-on: ubuntu-latest
    steps:
      - uses: 5monkeys/cobertura-action@master
        with:
          path: src/test.xml
          repo_token: ${{ secrets.GITHUB_TOKEN }}
          minimum_coverage: 75
```

## Development

- Install deps: `npm ci`
- Run tests: `npm run test`
- Run lint: `npm run lint`
- Package application `npm run package`. Remember to run this before committing anything.
