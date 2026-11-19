# GitHub Actions AI

A collection of AI-related GitHub actions.

## Actions

### Keep the self-review labels in step with the code

This action keeps two labels true to the code of a pull request after a self-review. The `pr-ready` skill of [aboutbits/agent-kit](https://github.com/aboutbits/agent-kit) writes a hidden `<!-- self-review: <sha> <branch-id> -->` marker into the PR body, and may set `risk:low`. The action computes the branch id the same way: a rebase keeps it, a change to the reviewed code does not.

- When the change differs from the reviewed one, it removes `risk:low` and adds `self-review:stale`.
- When the change is the reviewed one again, for example after a new self-review round, it removes `self-review:stale`.
- Without the marker, it removes both labels: the pull request has no self-review.
- On the top pull request of a stack that the self-review reviewed whole, the marker also names the stack's base: `<!-- self-review: <sha> <branch-id> base=<branch> -->`. Its `risk:low` covers the whole stack, so the action compares the change from that base, not from the pull request's own base. A change to a lower pull request reaches the top with the next restack, and then removes the label.
- One change can fire several events at once, such as `edited` and `labeled` from one `gh pr edit`. In the example's `concurrency` group, a newer run cancels the one still running for the same pull request, and each run reads the pull request as it is when it starts, not as the event saw it: the last run decides on the latest state. A label that is already gone counts as removed.

#### Example

```yaml
on:
  pull_request:
    types: [opened, reopened, synchronize, edited, labeled]

jobs:
  review-freshness:
    if: >-
      contains(github.event.pull_request.body, '<!-- self-review:')
      || contains(github.event.pull_request.labels.*.name, 'risk:low')
      || contains(github.event.pull_request.labels.*.name, 'self-review:stale')
    concurrency:
      group: review-freshness-${{ github.event.pull_request.number }}
      cancel-in-progress: true
    # runs-on: <the runner your other workflows use>
    permissions:
      contents: read
      pull-requests: write
    steps:
      - uses: aboutbits/github-actions-ai/github-pr-review-freshness@v2
```

#### Inputs

The following inputs can be used as `step.with` keys:

| Name          | Required/Default             | Description                                                |
|---------------|------------------------------|------------------------------------------------------------|
| `label`       | optional (risk:low)          | The label that allows a merge without more review          |
| `stale-label` | optional (self-review:stale) | The label that says the code changed after the self-review |

## Build & Publish

To build and publish the action, visit the GitHub Actions page of the repository and trigger the workflow "Release
Package" manually.

## Information

About Bits is a company based in South Tyrol, Italy. You can find more information about us
on [our website](https://aboutbits.it).

### Support

For support, please contact [info@aboutbits.it](mailto:info@aboutbits.it).

### Credits

- [All Contributors](../../contributors)

### License

The MIT License (MIT). Please see the [license file](license.md) for more information.
