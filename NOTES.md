# Notes

## What's in CLAUDE.md, and what I left out

I kept `CLAUDE.md` to the four parts that save Claude real exploring time:
- a one-line description
- the commands, including how to run a single test
- the architecture: the `server.js` entry point, one router per resource in `routes/`, and all data access through `db/store.js`
- the conventions

I left out the README's setup steps, a file-by-file listing and generic advice. Claude can find those in the code, or they'd just be noise loaded into every session. I also kept anything sensitive out: `CLAUDE.md` only points to `.env.example` and never includes real values.

## Permission rules

`.claude/settings.json` (committed, so the whole team gets the same rules):
- **allow** `Bash(npm test:*)`: tests are safe and run constantly, so asking for approval every time just slows things down.
- **ask** `Bash(git push:*)`: pushing sends my work to a shared remote and is hard to take back, so I want to confirm each push.
- **deny** `Read(./.env)`: `.env` is where real secrets would live.

Without the deny rule, Claude could open `.env` while debugging config. Any secrets in it would then end up in the conversation and in session logs, outside the place they're meant to stay. Git-ignoring `.env` keeps it out of commits, but it doesn't stop Claude from reading it.

## Verification

- `/memory` lists the project's `./CLAUDE.md` as loaded.
- `/permissions` shows the allow, ask and deny rules above.

