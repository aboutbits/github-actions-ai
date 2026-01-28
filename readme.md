# GitHub Actions AI

A collection of AI-related GitHub actions.

## Actions

### AI Code Review

Review a PR using Anthropic Claude Code GitHub action.

#### Example

```yaml
  - uses: aboutbits/github-actions-ai/code-review@v1
    with:
      anthropic-api-key: ${{ secrets.ANTHROPIC_API_KEY }}
```

#### Inputs

The following inputs can be used as `step.with` keys:

| Name                | Required/Default | Description       |
|---------------------|------------------|-------------------|
| `anthropic-api-key` | required         | Anthropic API Key |

#### Permissions

The action requires the following permissions:

- `contents: write`
- `pull-requests: write`
- `id-token: write`

#### Example: Workflow

This example shows how to trigger the action on a PR comment, which contains the word "ai review".

```
jobs:
  ai-review:
    if: |
      github.event.issue.pull_request != null &&
      contains(github.event.comment.body, 'ai review')
    runs-on: ubuntu-24.04
    timeout-minutes: 10
    permissions:
      contents: write
      pull-requests: write
      id-token: write

    steps:
      - name: Run AI Code Review
        uses: aboutbits/github-actions-ai/code-review@v1
        with:
          anthropic-api-key: ${{ secrets.ANTHROPIC_API_KEY }}
```

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
