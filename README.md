# Notes

## Daily
`./today`: opens `daily/YYYY-MM-DD.md`. Unfinished tasks and "Waiting on" items from the last note are carried over.
- Morning: pick the Top 3 and unblock others first.
- Big comment on a task? Indent it under the task (4 spaces). It carries over with the task.
- Evening: 3 lines in Log (what you learned or decided). These feed the weekly review.

## Weekly (Friday, 30 min)
`./today week`: opens `weekly/YYYY-Www.md`, prefilled with the last 7 days of logs.
Pick ONE thing to improve next week.

## Docs: write when you learn, not later
| Folder | When | Template |
|---|---|---|
| `docs/debug/` | Bug took > 1h | `templates/debug.md` |
| `docs/decisions/` | Chose X over Y | `templates/adr.md` |
| `docs/howto/` | Did it the 2nd time | — |
| `docs/hardware/` | Datasheet didn't say it | — |

See `sample/` for filled-in examples.

Find anything: `grep -ri "i2c" daily docs`
