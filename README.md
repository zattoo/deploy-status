# Deploy Action

GitHub Action to create GitHub Deployment, see https://developer.github.com/v3/repos/deployments

![environment](https://i.imgur.com/MTZ7noe.png)

## Inputs

### `token`

`string`

Required. GitHub token

### [`state`](https://developer.github.com/v3/repos/deployments/#list-deployment-statuses)

`"queued" | "pending" | "in_progress" | "error" | "failure" | "success"`

Required. Status of deploy

### `description`

`string`

A short description of the status. The maximum description length is 140 characters

### `environment`

`string`, default: `live`

Name for the target deployment environment. For example, "production", "release", or "qa"'


### `environment_url`

`string`

Sets the URL for accessing your environment

## Usage Example

````yaml
name: Delivery
on:
  push:
    branches:
      - master
jobs:
  deploy:
    name: deploy
    steps:
      - uses: actions/checkout@v2
      - name: init
        uses: zattoo/deploy-action@releases/v1
        id: deploy
        with:
          token: ${{ github.token }}
          environment: live
          environment_url: https://web.zattoo.com
          state: in_progress
      - run: # your delivery scripts
      - name: update success status
        if: success()
        uses: zattoo/deploy-action@releases/v1
        with:
          token: ${{ github.token }}
          environment: live
          environment_url: https://web.zattoo.com
          state: success
          deployment_id: ${{ steps.deployment.outputs.deployment_id }}
      - name: update failure status
        if: failure()
        uses: zattoo/deploy-action@releases/v1
        with:
          token: ${{ github.token }}
          environment: live
          environment_url: https://web.zattoo.com
          state: failure

````
