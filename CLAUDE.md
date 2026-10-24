# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

A small Express (Node.js, CommonJS) REST API for users, with an in-memory data store. It is the starter project for the Claude Code course.

## Commands

- `npm run dev`: start the API with auto-reload (`node --watch`) on `http://localhost:3000` (`PORT` overrides it)
- `npm test`: run all tests with the built-in Node test runner (`node --test`)
- `node --test tests/users.test.js`: run one test file; add `--test-name-pattern="404"` to run matching tests only
- `npm run lint`: run ESLint (`eslint:recommended`)

CI (`.github/workflows/ci.yml`, Node 22) runs `npm run lint`, then `npm test`, on every push and PR. Both must pass.

## Architecture

- `server.js` builds the Express app, mounts each router at its path, and exports `app`. It calls `listen()` only when run directly (`require.main === module`), so tests import `app` without opening a port.
- `routes/`: one file per resource (`users.js`, `health.js`), each exporting an `express.Router()` that `server.js` mounts.
- `db/store.js`: the only data-access layer. It keeps data in memory, so data resets on restart and is shared across tests in one process. Routes call store functions and never touch the data directly.
- `tests/`: `node:test` plus `node:assert` and `supertest` against the exported `app`.

## Conventions

- Use CommonJS (`require` / `module.exports`), not ES modules. ESLint is set to `sourceType: "script"`.
- Use `node:test` and `node:assert` for tests, not Jest or Mocha. Test through HTTP with `supertest(app)`.
- For a new resource, add `routes/<resource>.js`, mount it in `server.js`, and put its data access in `db/store.js`.
- Return errors as JSON `{ error: "<message>" }` with the right status (400 for bad input, 404 when not found). Return 201 for creates.
- Put a `// METHOD /path — description` comment above each route handler.
- Config comes from environment variables (see `.env.example`). Never read or commit `.env`.
