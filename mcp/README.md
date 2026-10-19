# MCP

Two servers, both Streamable HTTP without sessions.

| Server | Sign-in | For |
|---|---|---|
| `https://api.routinerun.com/mcp` | OAuth 2.1 — a code typed into the app | Your own routines and runs |
| `https://api.routinerun.com/mcp/public` | none | Writing a routine for somebody who has connected nothing, or has not got the app yet |

There is no account. Signing in to the first shows a short code; the person types it into
Routine Run under Settings › Connections › Connect an assistant, and the phone itself
approves the assistant. Removing that connection in the app signs it out at once. The
sign-in page also offers a demo library, for trying it without the app.

Every tool, what each does, and what the relay keeps: [developers/api.md](../developers/api.md#mcp).
How to add it to each assistant: [routinerun.com/help/assistants](https://routinerun.com/help/assistants).

## What it will and will not do

- Reads are answered by the phone, not by a copy on a server. A phone that does not answer
  in about 20 seconds is said to have not answered.
- A new routine, or a change to one, is a suggestion: it waits on the phone until the
  person saves it, unless they have set that assistant to save without asking.
- Starting, planning, pausing and moving runs happen at once, and each is undone with one
  press.
- Notes and values are read only where the person has turned on **Read notes and values**
  for that assistant.

## Registry entries

[`server.json`](server.json) and [`server-writer.json`](server-writer.json) are the entries
in the [official MCP registry](https://registry.modelcontextprotocol.io), as
`com.routinerun/routine-run` and `com.routinerun/routine-writer`.
