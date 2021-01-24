# ci-trigger
A github action to detect trigger keywords in the summary line of commit messages

# Usage

To use the detect-ci-trigger action in workflows, add a new job:

```yaml
  detect-ci-trigger:
    name: Detect CI Trigger
    runs-on: ubuntu-latest
    outputs:
      triggered: ${{ steps.detect-trigger.outputs.trigger-found }}
    steps:
    - uses: keewis/ci-trigger@v1
      id: detect-trigger
      with:
        keyword: "<keyword>"
```

then require the new job in jobs that should be conditionally skipped:

```yaml
  my-ci-job:
    runs-on: ubuntu-latest
    needs: detect-ci-trigger
    if: needs.detect-ci-trigger.outputs.triggered == 'false'  # for skipped ci
    # if: needs.detect-ci-trigger.outputs.triggered == 'true'  # for explicitly enabled ci
    steps:
    - actions/checkout@v2
    # ...
```