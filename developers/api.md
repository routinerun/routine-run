# Developers

A connection is an HTTPS address that starts or moves a routine on one person's iPhone. It is made in the app, under Settings › Connections, and this page is what a script, a pager or an automation sends to it.

The OpenAPI file, the MCP registry entries, examples and a Claude Code plugin are also on [GitHub](https://github.com/routinerun/routine-run), which is where an issue about any of this goes.

## The address and the secret

Each connection has its own address and its own secret:

```
https://api.routinerun.com/v1/in/<connection id>
Authorization: Bearer rr_…
```

The secret is shown in the app, never by the relay, which keeps only a fingerprint of it. Every request below goes to the address with one of the paths after it, and a JSON body.

There is no account and no API key for the whole service: an address reaches the one phone that made it, and nothing else. Removing the connection in the app stops the address at once.

## What you can send

| Request | Body | What the phone does |
|---|---|---|
| `POST /events` | `title`, and `body`, `source`, `externalId`, `routine`, `fields` if you have them | What **Handle Event** does: the person's Sources decide which routine starts |
| `POST /runs` | `routine` — a name or an id; `name` and `values` (an object of key → value) for the run; `restart: true`; `askFirst: true` to wait for the person's OK | Starts that routine, or puts Start and Drop in In hand. A routine that runs one at a time and is already going answers `alreadyGoing` unless `restart` is said |
| `POST /runs` | `written` — a routine in the [written form](https://routinerun.com/llms-routine.txt), and `askFirst` | Starts a run of those steps, kept out of Routines unless the person keeps it |
| `POST /runs` | `routine` and `at` — an ISO 8601 moment to come; `name`, `values` and `askFirst` | Plans a run of that routine; it shows as Planned and begins then |
| `POST /runs/{run}/next` | `expectsStep` if you know it | Moves the run on, unless it has moved since |
| `POST /runs/{run}/pause`, `/resume`, `/stop` | nothing | Pauses, resumes or stops the run, as its own buttons do |
| `POST /runs/{run}/later`, `/last` | `expectsStep` if you know it | Puts the step in hand one place down, or at the end |
| `POST /runs/{run}/notes` | `text` | Writes a note on the run, at the step it is on |
| `PUT /runs/{run}/values/{key}` | `value` | Sets a value on the run |
| `POST /routines/{routine}/steps` | `title`, and `detail` | Adds a step to a list that stays open |
| `POST /pagerduty` | a PagerDuty Webhooks V3 delivery, as sent | Reads the incident as an event |

### What you can read

| Request | What comes back |
|---|---|
| `GET /routines`, `?match=` part of a name | Each routine's id, name, step count and whether it stays open as a list |
| `GET /routines/{routine}` | Its steps, its version and its written form |
| `GET /runs` | The runs going, and the step each is on |
| `GET /runs/{run}` | One run's steps, each done, in hand or ahead — never its notes or values |
| `GET /history`, `?days=` up to 92 and `?routine=` | Runs finished, stopped and going per routine, and the steps most often skipped or put off |

A read is answered by the phone, so it needs the phone reachable; one that does not answer in about 20 seconds is `504 phone_did_not_answer`.

A routine is found by its id, or by its name in any case. Two routines with the same name are neither.

For example:

```
curl -X POST https://api.routinerun.com/v1/in/<connection id>/runs \
  -H "Authorization: Bearer rr_…" \
  -H "content-type: application/json" \
  -d '{"routine": "Morning"}'
```

## What comes back

Every request is answered at once with `202` and a command id:

```
{"command": "896ea5dc-1477-4edd-8a77-18333032f8f6", "accepted": true}
```

`202` means the relay handed it to Apple for the phone, not that it was done. The phone is the only place that knows the routines, so only the phone can say. Ask it:

```
GET /v1/in/<connection id>/commands/<command id>
```

`status` is one of the words below, and where the command started or found a run, `run` is its id — what `/runs/{run}/…` takes next:

```
{"command": "896ea5dc-…", "status": "done", "run": "3f0c2b1e-…"}
```

| Status | Meaning |
|---|---|
| `waiting` | The phone has not answered yet, or the answer is more than an hour old |
| `done` | Carried out |
| `refused` | The phone would not: a step that is not finished, a routine that runs one at a time, a policy |
| `tooOld` | Arrived after it stopped meaning anything |
| `runHadMoved` | A Next that expected another step |
| `alreadyDone` | The same command, again |
| `notFound` | No such routine or run on the phone |
| `alreadyGoing` | A start of a routine that runs one at a time, with a run of it going: nothing started or ended, and `run` is the one going |

## How long a command waits

A command is not done if the phone gets it after this long:

| Command | Waits |
|---|---|
| Next | 15 minutes |
| Start | 1 hour |
| Events, notes, values, steps | 24 hours |

On iOS 27 with the Routine Run bridge, a command is carried out within seconds, with the phone locked. On iOS 26, or without the bridge, it is carried out when the person taps the notification or opens the app.

## Errors

| Status | `error` | Meaning |
|---|---|---|
| 401 | `secret_needed` | No `Authorization: Bearer` header |
| 401 | `unknown_address_or_secret` | The connection is gone, or the secret is wrong |
| 400 | `title_needed`, `routine_needed`, `text_needed` | The body is missing what that request needs |
| 404 | `not_found` | No such path |
| 409 | `phone_has_no_push_token` | The phone has not been reachable yet |
| 410 | `phone_gone` | The app was deleted from the phone |
| 413 | `body_too_long` | Bodies are 3000 bytes at most |
| 429 | `too_many_requests` | 60 a minute for each address |

An address belongs to the copy of the app that made it. When a different build replaces it on the phone — a TestFlight build over one from Xcode, or the other way round — the phone gets a new push token, and older addresses answer `410` or are accepted and never arrive. Make the connection again on the phone and use the new address and secret.

## MCP

The same connection, for an assistant: `https://api.routinerun.com/mcp`, Streamable HTTP without sessions. Sign-in is OAuth 2.1 with PKCE (S256), clients registered dynamically or named by a client metadata URL; the metadata is at `/.well-known/oauth-protected-resource/mcp` and `/.well-known/oauth-authorization-server`. Signing in shows a code the person types on their phone, or offers a demo library. How each assistant adds it: [Connect Claude, ChatGPT and others](https://routinerun.com/help/assistants).

| Tool | What it does |
|---|---|
| `list_routines`, `get_routine` | Names and step counts; one routine's steps, its version and its written form |
| `list_runs`, `get_run` | The runs going, and where each stands |
| `start_run`, `next_step`, `pause_run`, `resume_run`, `stop_run`, `put_off_step`, `add_note`, `set_value`, `add_step` | The requests above, waiting up to 15 seconds for the phone's answer |
| `get_history` | How the last few days went: runs finished, stopped and going for each routine, the steps most often skipped or put off — never notes or values |
| `propose_routine_change` | A routine whole in its written form, shown on the phone to be saved or dropped |
| `plan_run` | Plans a run of a routine for a moment to come; it shows as Planned and begins then |
| `how_to_write_a_routine`, `routine_link`, `find_starter_routine` | The public tools below, here too, so one connector is enough |
| `start_new_run` | A run of steps the assistant writes, begun at once and kept out of Routines unless the person keeps it |
| `propose_new_routine` | A new routine in the written form, with no `from`, shown on the phone to be saved or dropped |
| `command_status` | What became of a command |

`start_run`, `start_new_run` and `plan_run` take `ask_first`: the run then waits in In hand for the person's Start, and the receipt is `asked` until they answer — `done` or `refused`. A connection set to **Ask before starting** does this for every run. `get_run` carries the run's notes and values only where the person has turned on **Read notes and values** for that assistant. The server also offers prompts — `review_my_week`, `plan_tomorrow`, `checklist_now`, `tidy_a_routine`, `where_am_i` — which clients show as commands.

Reads are answered by the phone, so they need it reachable; a phone that does not answer in about 20 seconds is said to have not answered.

### Writing a routine, with no sign-in

`https://api.routinerun.com/mcp/public` is a second MCP server for an assistant helping somebody who has connected nothing, or does not have the app yet. Nothing on it is anybody's, so it needs no sign-in.

| Tool | What it does |
|---|---|
| `how_to_write_a_routine` | The form the app reads, the same text as [llms-routine.txt](https://routinerun.com/llms-routine.txt) |
| `routine_link` | Checks a routine in that form and returns a `routinerun.com/w?d=…` link that opens it on the app's review screen |
| `find_starter_routine` | The routines the app ships, found by words, each with its steps and a link that adds it |

The routine sent to `routine_link` is turned into a link and not kept. 60 calls a minute from one address.

## PagerDuty

In PagerDuty, add a Generic Webhook (V3) subscription to the service, with the address followed by `/pagerduty` and a custom header `Authorization` set to `Bearer` and the secret. An incident's title becomes the event's title and its id the event's id, so a page that is sent twice starts one run.

## What the relay keeps

The connection's address and name, when it was last used, a token for reaching the phone, and a fingerprint of the secret. Not the body of anything you send: it is passed to the phone and not kept, and Apple holds it until the phone is reachable, a day at most. The phone's answer, one word, is kept for about an hour. The [privacy page](https://routinerun.com/privacy) says the rest.

The whole of it in OpenAPI: [openapi.json](https://routinerun.com/developers/openapi.json).
