# Force-Label
Github Action that forces developers to add labels to their PR before it is merged.

## Required Labels
To have multiple required labels you can use `||`:

```
jobs:
  check-labels:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    - name: Labels not added (bug, documentation, enhancement)
      if: >
        contains(github.event.pull_request.labels.*.name, 'bug') == false ||
        contains(github.event.pull_request.labels.*.name, 'allocations') == false ||
        contains(github.event.pull_request.labels.*.name, 'fe') == false
      run: exit 1
```

## Layered Labels
To require labels specific to parent labels you can use `needs`

```
jobs:
  check-bug:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    - name: Labels not added (bug)
      if: >
        contains(github.event.pull_request.labels.*.name, 'bug') == false && 
      run: exit 1

  check-sev:
    runs-on: ubuntu-latest
    needs: check-bug
    steps:
    - uses: actions/checkout@v3
    - name: Labels not added (sev1, sev2, sev3)
      if: >
        contains(github.event.pull_request.labels.*.name, 'sev1') == false && 
        contains(github.event.pull_request.labels.*.name, 'sev2') == false && 
        contains(github.event.pull_request.labels.*.name, 'sev3') == false
      run: exit 1
```
