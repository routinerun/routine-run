# Contributing

Thank you for looking. Most of what is here is copied out of the repository the app is
built from, and changes there in the same commit as the app it describes — so a help page
can never say a button is somewhere it no longer is. That decides how you can help.

## What helps most

**An issue.** A bug, a page that is wrong, a routine you would like, an app whose
notifications should start a routine, a service a step should be able to send to. The
forms ask for what we need. [Open one](https://github.com/routinerun/routinerun/issues/new/choose).

**The real wording of an app's notifications.** The [connections](connections/README.md)
work by reading what another app's notification says, and for most apps we have not seen
the real text yet. Connect **Any app** in Routine Run (Settings › Connect an app), let
the app send you a few, and copy the App, Title and Body of each from History › Decisions
into the *Connect Routine Run to an app or a system* form. That is the whole of the work;
we write the file and the test from it. Leave out anything personal in the text.

## Pull requests

Not yet, for the files that are copied in — which, for now, is all of them. A pull request
against one is closed with a note asking for an issue instead, because the change has to
be made where the file comes from. It is not a judgement on the change.

Folders called `community/` will be the exception: routines, shortcuts and presets that
other people write, reviewed here and owned here. They open once there is a way to check
each one properly; until then, the issue forms are the way in.

## What anything sent here has to keep to

- **Nothing leaves the phone without the person saying so.** In the app, a webhook, an
  event, a reminder, an alarm, a Home scene and a Health entry each arrive switched off
  until the person allows them. A routine, a shortcut or a preset says what it sends and
  where, and sends nothing else.
- **No secrets.** A token, a key or the end of a webhook address is written as
  `{secret:name}`; the person pastes the real one on their phone.
- **Only the service's own addresses.** A preset for a service sends to the addresses that
  service documents.
- **English**, for routines: a routine the app hands somebody becomes theirs, in the words
  it was written in.
- **Say what it depends on.** A routine that needs a shortcut, an app or Health says so in
  its `about`, which is what somebody reads before adding it.
- **No medical or legal promises.**

## Licence of what you send

What you send is under the licence of the folder it goes into — CC BY 4.0 for text, MIT
for code and schemas, CC0 for catalogue entries — and by sending it you confirm you are
entitled to give it under that licence. Sign your commits off (`git commit -s`).

## Code of conduct

[CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md). In short: be kind, and assume the person on the
other side is having the kind of day this app was built for.
