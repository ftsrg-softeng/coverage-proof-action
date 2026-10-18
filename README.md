# coverage-proof

Runs on `ubuntu-latest` (or another Ubuntu LTS runner) only.

Licensed under [Apache License 2.0](LICENSE).

After `mvn verify`, checks the JaCoCo coverage of one class and the number
of passing, asserted tests in one surefire suite against the thresholds in
[`config.env`](config.env), then comments the results on the pull request.
When every criterion is met, the comment ends with an exit-criterion string
to paste into Moodle. Later runs edit that same comment rather than adding
new ones.

## Usage

Add it as a step after `mvn verify`, in a job that runs on `pull_request`:

```yaml
permissions:
  pull-requests: write

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      # ... checkout, setup-java, mvn verify ...
      - name: Post exit-criterion string
        uses: ftsrg-softeng/coverage-proof-action@master
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
```

The step stays green when a criterion is not met (the comment says which
one) and fails only when the reports cannot be read.
