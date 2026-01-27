# GitHub Actions AI

A collection of AI-related GitHub actions.

## Actions

### AI Code review

Review a PR using Anthropics Claude Code GitHub action.

#### Example

```yaml
  - uses: aboutbits/ai-code-review@v1
    with:
      anthropic-api-key: ${{ secrets.ANTHROPIC_API_KEY }}
```

#### Inputs

The following inputs can be used as `step.with` keys:

| Name                | Required/Default | Description       |
|---------------------|------------------|-------------------|
| `anthropic-api-key` | required         | Anthropic API Key |

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
