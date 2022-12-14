# GitHub Actions for flyctl

This action wraps the flyctl CLI tool to allow deploying and managing fly apps. We recommend using the `setup-flyctl` action which runs outside of Docker for speed and flexibility.
## Usage for deployment

```yaml
name: Deploy to Fly
on: [push]
jobs:
  deploy:
    name: Deploy proxy
    runs-on: ubuntu-latest
    steps:
      # This step checks out a copy of your repository.
      - uses: actions/checkout@v2
      - uses: jmoll/setup-flyctl@1.1.1
      - run: flyctl deploy --remote-only
        env:
          FLY_API_TOKEN: ${{ secrets.FLY_API_TOKEN }}
```

To use a specific version of `flyctl`:

```yaml
- uses: jmoll/setup-flyctl@1.1.1
  with:
    version: 0.0.308
```
### Run one-off scripts

```yaml
name: Run on Fly
on: [push]
jobs:
  deploy:
    name: Run script
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: jmoll/setup-flyctl@1.1.1
      - run: "flyctl ssh console --command 'sh ./myscript.sh'"
        env:
          FLY_API_TOKEN: ${{ secrets.FLY_API_TOKEN }}
```

See the [flyctl](https://github.com/superfly/flyctl) GitHub project for more information on using `flyctl`.

## Secrets

`FLY_API_TOKEN` - **Required**. The token to use for authentication. You can find a token by running `flyctl auth token` or going to your [user settings on fly.io](https://fly.io/user/personal_access_tokens).

## Using the `setup-flyctl` action

This documentation only covers usage of the `setup-flyctl` action. The main action in this repository runs more slowly inside Docker, so is not recommended. It's been kept in place since many CI pipelines are still using it.
