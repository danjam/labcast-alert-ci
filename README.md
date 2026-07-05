# labcast-alert-ci

Composite GitHub Action that notifies labcast when a CI workflow fails. Connects to Tailscale and POSTs failure details to the labcast webhook bridge.

## Usage

```yaml
- uses: danjam/labcast-alert-ci@master
  if: failure()
  continue-on-error: true
  with:
    labcast-url: ${{ secrets.LABCAST_URL }}
    labcast-api-key: ${{ secrets.LABCAST_API_KEY }}
    ts-oauth-client-id: ${{ secrets.TS_OAUTH_CLIENT_ID }}
    ts-oauth-secret: ${{ secrets.TS_OAUTH_SECRET }}
```

Use `continue-on-error: true` so notification infrastructure failures don't change the workflow outcome.

## Inputs

| Input | Description |
|---|---|
| `labcast-url` | Labcast webhook bridge URL |
| `labcast-api-key` | Labcast API key |
| `ts-oauth-client-id` | Tailscale OAuth client ID |
| `ts-oauth-secret` | Tailscale OAuth secret |

## Payload

Published to `alerts/ci` MQTT topic:

```json
{
  "repo": "danjam/labcast",
  "workflow": "Build labcast image",
  "branch": "master",
  "commit": "abc1234...",
  "run_url": "https://github.com/danjam/labcast/actions/runs/12345"
}
```
