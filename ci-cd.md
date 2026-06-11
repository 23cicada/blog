## GitHub Actions

- [Understanding GitHub Actions](https://docs.github.com/en/actions/get-started/understand-github-actions)

## Actions

`actions/checkout`: It checks out the project source code from Git.

`setup-node`: Set up your GitHub Actions workflow with a specific version of node.js

```yaml
- uses: actions/setup-node@v6        
  with:          
    node-version: '24'
```
The `with` keyword is used to give a "parameter" to the action.

`upload-artifact`: uploads files generated during a GitHub Actions workflow so they can be downloaded later or shared between jobs.

```yaml
- uses: actions/upload-artifact@v4
  if: ${{ !cancelled() }}
  with:
    name: playwright-report
    path: playwright-report/
    retention-days: 30
```

## Webhook events and payloads

[push](https://docs.github.com/en/webhooks/webhook-events-and-payloads#push)

[pull_request](https://docs.github.com/en/webhooks/webhook-events-and-payloads#pull_request)



## Pull Requests

```yaml
pull_request:
  branches: [main]
  types: [opened, synchronize]
```
