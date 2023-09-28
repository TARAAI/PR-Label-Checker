# Force-Label
Github Action that forces developers to add labels to their PR before it is merged.

# Customize Labels
To customize labels, simply add/remove from the list:
```
      if: >
        contains(github.event.pull_request.labels.*.name, 'bug') == false && 
        contains(github.event.pull_request.labels.*.name, 'documentation') == false && 
        contains(github.event.pull_request.labels.*.name, 'enhancement') == false &&
        contains(github.event.pull_request.labels.*.name, 'newlabel1') == false &&
        contains(github.event.pull_request.labels.*.name, 'newlabel2') == false
      run: exit 1
```
This will require either `bug`, `documentation`, `enhancement`, `newlabel1`, or `newlabel2` to be attached to a PR.

## Required Labels
To have multiple required labels on a single PR you can use `||`:

```
      if: >
        contains(github.event.pull_request.labels.*.name, 'bug') == false ||
        contains(github.event.pull_request.labels.*.name, 'allocations') == false ||
        contains(github.event.pull_request.labels.*.name, 'fe') == false
      run: exit 1
```

## Related Labels
To require labels related to other labels you can use `needs` on a specific job.

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
