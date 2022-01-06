# dignifi-ui-deploy

This is a composite Github Action that is used deploy UI assets to AWS S3 and Cloudfront. 

See the confluence page for more information about setting up CI/CD for UI applications.


### TODO

Since composite actions must be contained in public repositories and cannot contain secrets yet, all secrets must be passed in to the action as arguments.

In the future, this repository should be made private and the organizational secrets should be referenced directly in the action instead of through parameters.

See these issues for tracking:

* https://github.com/actions/runner/issues/646
* https://github.com/github/roadmap/issues/74

### Usage

|parameter|description|required|default|secret|
|-|-|-|-|-|
|rails-env|rails backend environment|no|staging1|no|
|bucket|AWS S3 bucket|yes|-|yes|
|distribution|AWS Cloudfront distribution|yes|-|yes|
|access-key-id|AWS Access Key Id|yes|-|yes|
|secret-access-key|AWS Secret Access key|yes|-|yes|
|npm-token|DigniFi NPM token|yes|-|yes|

The AWS region will always be `us-west-2`.

```yaml
name: staging environments deploy
on:
  push:
    branches:
      - main
jobs:
  staging1:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: cfscode/dignifi-ui-deploy@main
        with:
          rails-env: 'staging1'
          bucket: ${{ secrets.AWS_STAGING_1_S3_BUCKET }}
          distribution: ${{ secrets.AWS_STAGING_1_DISTRIBUTION }}
          access-key-id: ${{ secrets.ORG_AWS_ACCESS_KEY_ID }}
          secret-access-key: ${{ secrets.ORG_AWS_SECRET_ACCESS_KEY }}
          npm-token: ${{ secrets.ORG_NPM_TOKEN }}
```
