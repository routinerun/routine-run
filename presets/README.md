# Presets: services a step can send to

One file per service. A preset is the address a service takes, the body it wants, the
fields somebody fills in and the secrets they keep on their phone — written down once, so a
step can say "post this to Discord" without anybody reading Discord's documentation.

The app reads these files at build time; nothing is fetched. What a preset makes is an
ordinary webhook, so a step made from one arrives switched off like any other and asks for
its key the first time. In the written form a model uses, each preset is a word of its own:
`{"discord": {"text": "{instance} is done"}}`.

Home Assistant, Slack, ntfy, PagerDuty and IFTTT are not here. Each has something a
template cannot say — PagerDuty's trigger needs a summary and its acknowledge a key, Home
Assistant's address is somebody's own — so they are written in the app's code.

## The file

| field | what it is |
|---|---|
| `id` | lower case, one word: the file name, and the word in the written form |
| `service` | the service's name |
| `says` | what a step's row says it does: "Post to Discord" |
| `summary` | one sentence: what happens |
| `docs` | the service's own page for this request |
| `fields` | what somebody fills in: `key`, `optional`, and an `example` a new step starts with |
| `secrets` | each secret the request names: `name`, what it `is`, and `where` to find it |
| `request` | `method`, `url`, `headers` and `body`. `{field:key}` fills a whole string of the body; `{secret:name}` stays a name, and the phone puts the key in when it sends |
| `sends` | what leaves the phone, in a sentence: the line somebody reads before Allow |
| `hosts` | where it goes. The request's address must be one of these |
| `samples` | requests it must make, with the fields that make them. Every one is checked before a build |

A field fills a whole string and nothing else, so a request can always be read back into
the preset that made it. The fields the app has words for are `text`, `title` and `chat`.

## What a preset has to keep to

- It goes only to the service's own documented address, and `hosts` says which.
- It says what it sends, and sends nothing else.
- No secret is ever written out: a token, a key, the end of a webhook address is
  `{secret:name}`, explained in `secrets`.

Licence: [CC0](../LICENSE-DATA).
