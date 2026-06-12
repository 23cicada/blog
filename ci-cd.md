## GitHub Actions

- [Understanding GitHub Actions](https://docs.github.com/en/actions/get-started/understand-github-actions)

## Actions

`actions/checkout`: Checks out the project source code from the repository.

`setup-node`: Sets up your GitHub Actions workflow with a specific version of Node.js.

```yaml
- uses: actions/setup-node@v6
  with:
    node-version: '24'
```
The `with` keyword is used to pass parameters to the action.

`github-tag-action`: A GitHub Action that tags the repository on merge.

`upload-artifact`: Uploads files generated during a workflow run so they can be downloaded later or shared between jobs.

```yaml
- uses: actions/upload-artifact@v4
  if: ${{ !cancelled() }}
  with:
    name: playwright-report
    path: playwright-report/
    retention-days: 30
```

## Webhook events and payloads

[Push Event](https://docs.github.com/en/webhooks/webhook-events-and-payloads#push)

[Pull Request Event](https://docs.github.com/en/webhooks/webhook-events-and-payloads#pull_request)

## Pull Requests

```yaml
pull_request:
  branches: [main]
  types: [opened, synchronize]
```

Run the deployment step only for the main branch: this ensures that deployment happens only when code is pushed or merged into the main branch.

`if: ${{ github.event_name == 'push' }}`

## Versioning

```yaml
- uses: actions/checkout@v6
  with:
    # merge_commit_sha: the SHA of the merge commit that GitHub automatically creates for the pull request.
    ref: ${{ github.event.pull_request.merge_commit_sha }}
    # Fetch the full Git history instead of performing a shallow clone.
    fetch-depth: '0'

- name: Bump version and push tag
  uses: anothrNick/github-tag-action@1.75.0
  env:
    TAG_PREFIX: "v"
    DEFAULT_BUMP: "patch"
    GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

## Skipping a commit for tagging and deployment

[`join`](https://docs.github.com/en/actions/reference/workflows-and-actions/expressions#join)

[`contains`](https://docs.github.com/en/actions/reference/workflows-and-actions/expressions#contains)

[`github.event.commits`](https://docs.github.com/en/webhooks/webhook-events-and-payloads#push)

```yaml
if: ${{ github.event_name == 'push' && !contains(join(github.event.commits.*.message), '#skip') }}
```

## Keep the main branch protected

[Managing a branch protection rule](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/managing-a-branch-protection-rule)

[About status checks](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/collaborating-on-repositories-with-code-quality-features/about-status-checks)

## Periodic tasks

Actions:

[`URL Health Check`](https://github.com/marketplace/actions/url-health-check)

[`schedule`](https://docs.github.com/en/actions/reference/workflows-and-actions/events-that-trigger-workflows#schedule) - Triggers a workflow at a scheduled time.
```yaml
on:
  schedule:
    - cron: "15 4,5 * * *"
```

[`workflow_dispatch`](https://docs.github.com/en/actions/reference/workflows-and-actions/events-that-trigger-workflows#workflow_dispatch) - Enables a workflow to be triggered manually.

```yaml
on: workflow_dispatch
```

## Example

```yaml
name: Pipeline

on:
  push:
    branches:
      - main
  pull_request:
    branches: [main]
    types: [opened, synchronize]

jobs:
  build_and_test:
    runs-on: ubuntu-latest
    defaults:
      run:
        working-directory: part11/fs-pokedex-main
    steps:
      - uses: actions/checkout@v6
      - uses: actions/setup-node@v6
        with:
          node-version: "24"

      - name: Install dependencies
        run: npm install

      - run: npm run eslint

      - run: npm test

      - name: Install Playwright Browsers
        run: npx playwright install --with-deps chromium

      - run: npm run test:e2e

      - uses: actions/upload-artifact@v4
        if: ${{ !cancelled() }}
        with:
          name: playwright-report
          path: part11/fs-pokedex-main/playwright-report/
          retention-days: 30

      - run: npm run build

  deploy:
    if: ${{ github.event_name == 'push' && !contains(join(github.event.commits.*.message), '#skip') }}
    needs: [build_and_test]
    runs-on: ubuntu-latest
    defaults:
      run:
        working-directory: part11/fs-pokedex-main
    steps:
      - uses: actions/checkout@v6
      - uses: superfly/flyctl-actions/setup-flyctl@master
      - run: flyctl deploy --remote-only
        env:
          FLY_API_TOKEN: ${{ secrets.FLY_API_TOKEN }}

  tag_release:
    needs: [deploy]
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v6
        with:
          ref: ${{ github.event.pull_request.merge_commit_sha }}
          fetch-depth: "0"

      - name: Bump version and push tag
        uses: anothrNick/github-tag-action@1.75.0
        env:
          TAG_PREFIX: "v"
          DEFAULT_BUMP: "patch"
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}

  debug_github_context:
    if: ${{ github.event_name == 'push' }}
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: github context
        env:
          GITHUB_CONTEXT: ${{ toJson(github) }}
        run: echo "$GITHUB_CONTEXT"
      - name: commits
        env:
          COMMITS: ${{ toJson(github.event.commits) }}
        run: echo "$COMMITS"
      - name: commit messages
        env:
          COMMIT_MESSAGES: ${{ toJson(github.event.commits.*.message) }}
        run: echo "$COMMIT_MESSAGES"
```


```yaml
name: Periodic Health Check

on:
  workflow_dispatch:
  schedule:
    - cron: '*/15 * * * *'

jobs:
  health_check:
    runs-on: ubuntu-latest
    steps:
      - name: URL Health Check
        uses: jtalk/url-health-check-action@v5
        with:
          url: https://fs-pokedex-main-gentle-wind-6649.fly.dev/health
```