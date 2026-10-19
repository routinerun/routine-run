# Examples

Each starts a routine called Morning on the phone that made the connection. Replace
`<connection id>` and `rr_…` with the address and secret Settings › Connections showed you
once. Every request is answered at once with `202` and a command id; the phone does the
rest. All of it: [api.md](../api.md).

Never put the secret in a repository, an issue or a shared shortcut. If it has been
anywhere public, remove the connection and make a new one.

## curl

```sh
curl -X POST "https://api.routinerun.com/v1/in/$RR_CONNECTION/runs" \
  -H "Authorization: Bearer $RR_SECRET" \
  -H "content-type: application/json" \
  -d '{"routine": "Morning"}'
```

## Python

```python
import json, os, urllib.request

request = urllib.request.Request(
    f"https://api.routinerun.com/v1/in/{os.environ['RR_CONNECTION']}/runs",
    data=json.dumps({"routine": "Morning"}).encode(),
    headers={"Authorization": f"Bearer {os.environ['RR_SECRET']}", "content-type": "application/json"},
    method="POST",
)
with urllib.request.urlopen(request) as response:
    print(response.status, json.load(response))
```

## Node

```js
const response = await fetch(`https://api.routinerun.com/v1/in/${process.env.RR_CONNECTION}/runs`, {
  method: 'POST',
  headers: { Authorization: `Bearer ${process.env.RR_SECRET}`, 'content-type': 'application/json' },
  body: JSON.stringify({ routine: 'Morning' }),
});
console.log(response.status, await response.json());
```

## Home Assistant

In `configuration.yaml`, with the address and the secret in `secrets.yaml`:

```yaml
rest_command:
  routine_run_start:
    url: !secret routine_run_runs_url        # https://api.routinerun.com/v1/in/<connection id>/runs
    method: POST
    headers:
      Authorization: !secret routine_run_bearer   # "Bearer rr_…"
    content_type: application/json
    payload: '{"routine": "{{ routine }}"}'
```

Then an automation calls `rest_command.routine_run_start` with `routine: Good night`.

## Plan a run for later

```sh
curl -X POST "https://api.routinerun.com/v1/in/$RR_CONNECTION/runs" \
  -H "Authorization: Bearer $RR_SECRET" \
  -H "content-type: application/json" \
  -d '{"routine": "Leaving for the airport", "at": "2026-11-02T06:30:00-05:00", "askFirst": true}'
```

`askFirst` puts Start and Drop in In hand instead of starting by itself.
