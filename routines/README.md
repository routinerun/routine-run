# Routines

The routines Routine Run ships, one file each, exactly as the app's own catalogue writes
them for [routinerun.com/routines](https://routinerun.com/routines).

- [`starters/`](starters) — mornings, evenings, the house, work, health, the year.
- [`on-call/`](on-call) — runbooks for being paged: the first minutes of an incident, handing
  the pager over, after it is fixed. These are what a [connection](../connections/README.md)
  from PagerDuty or Opsgenie starts.

Each file has `steps`, the routine's `description` (which says what it depends on — a
shortcut, Health, a timer that has to reach you), and `link_web`, which opens the routine
in the app, where nothing is saved until you press Add.

A routine is English on purpose: once it is added it is that person's routine, in the words
it was written in.

To write one of your own for the app, use [the form](../plugin/skills/routine-run/SKILL.md) or the
public MCP server. Routines other people write will go in `community/` once there is a way
to review them; until then, [ask for one](https://github.com/routinerun/routinerun/issues/new?template=routine.yml).

Licence: [CC BY 4.0](../LICENSE-CONTENT).
