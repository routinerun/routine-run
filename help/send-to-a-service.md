# Send to a service

When a step is reached, it can post to Discord, message you on Telegram, push through
Pushover or Google Chat, ping a Healthchecks.io check or start a Zap — without you
reading anybody's documentation.

## Setting one up

1. On the step, add an action and choose **Call a webhook**.
2. Press **Send to a service…** and pick the service.
3. Write the message. It can carry what the run knows, like its name or the step it is on.
4. Press **Add the secret…** and paste the key. The page says what the key is and where
   that service shows it. It is kept in this phone's keychain; the routine holds only its
   name, so it is never in an export, a shared link or anything an assistant is shown.

It sends when the step is reached, to that service and nowhere else. One written by an
assistant arrives switched off instead: nothing is sent until you have looked at it on
the step's page and pressed **Allow**.

## Two channels of one service

Each service keeps its key under one name, such as `discord`. For a second channel, ask
your assistant for the step with its own name for the key — `"secret": "discord-team"` —
and add that key the same way.

## What leaves the phone

Each service's page says it in a line: the message, to that service, and nothing else.
Changing the request by hand under **The request** turns the step back into a plain
webhook, which is then exactly what you wrote.

Home Assistant, Slack, ntfy, PagerDuty and IFTTT are steps an assistant writes for you
by name; they are not in this list. Want a service that is not here? Write to
support@routinerun.com.
