# UNIX Signals — SIGINT, SIGTERM, SIGKILL (concise)

This short note explains the common process signals you’ll encounter and when to use them.

## Overview
- **SIGINT (2)** — interactive interrupt (usually `Ctrl-C`). Sent by the terminal to the foreground process group. Catchable: program can handle it (cleanup, prompt, ignore).
- **SIGTERM (15)** — polite termination request (default for `kill PID`). Sent by other programs or service managers. Catchable: allows graceful shutdown.
- **SIGKILL (9)** — forceful kill. Uncatchable and cannot be ignored; kernel stops the process immediately.

## Key differences
- Intent: SIGINT = user interrupt; SIGTERM = request to terminate cleanly; SIGKILL = immediate stop.
- Handling: SIGINT and SIGTERM can be caught/handled by the process; SIGKILL cannot.
- Effects: Handlers for SIGINT/SIGTERM may close files, flush state, or refuse exit; SIGKILL prevents any handlers.

## Common commands
- Send SIGTERM (polite):

  kill <pid>

- Send SIGINT (like Ctrl-C):

  kill -SIGINT <pid>

- Send SIGKILL (force):

  kill -9 <pid>

- List available signals: `kill -l`

## Sending to process groups
Terminal `Ctrl-C` targets the foreground process group. To mimic that from the shell:

- Send SIGINT to a process group (pgid): `kill -SIGINT -<pgid>` (note leading minus)
- Find a process's PGID: `ps -o pid,pgid,cmd -p <pid>` or `ps -o pgid= -p <pid>`

## Best practices
- Try polite first: `kill <pid>` (SIGTERM). Let services/daemons handle shutdown.
- If the process ignores or hangs, escalate to `kill -9 <pid>` (SIGKILL) as a last resort.
- When automating shutdowns, prefer SIGTERM so apps can cleanup; reserve SIGKILL for stubborn processes.

## Short example

- Graceful: `kill 1234` → app receives SIGTERM and exits cleanly.
- Interactive: press `Ctrl-C` while a foreground process runs → it receives SIGINT.
- Force: `kill -9 1234` → kernel immediately reclaims the process.

If you want, I can add a one-line summary to `00-cmd.md` linking to this file.
