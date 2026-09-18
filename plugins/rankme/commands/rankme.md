---
name: rankme
description: Rank yourself as a coder, from the AI coding sessions already on this machine
argument-hint: "[--all-repos]"
allowed-tools: Bash(command -v:*), Bash(test:*), Bash(curl:*), Bash(rankme discover:*), Bash(rankme analyze:*), Bash(~/.local/bin/rankme discover:*), Bash(~/.local/bin/rankme analyze:*)
---

# Rank me as a coder

Reads the Claude Code, Cursor, Codex and opencode sessions already on this machine,
measures them **locally**, and shows the evidence. Nothing is uploaded by anything
below.

## Say this first, before running anything

Tell the user, in one line: **this session cannot publish, and that is deliberate.**

`builder publish` asks "Publish this score to the public leaderboard?" and that prompt
is marked irreversible — it reads `/dev/tty`, refuses when there is no terminal, and
cannot be satisfied by `--yes`. A Bash tool has no controlling terminal. So everything
up to the question happens here, and the question itself happens in their own terminal.

State it up front. Do not discover it by failing.

## Steps

1. **Is it installed?**

   ```
   command -v rankme || test -x ~/.local/bin/rankme
   ```

2. **If not, install it.**

   ```
   curl -fsSL https://builder-production-5049.up.railway.app/run.sh | sh -s -- --install-only
   ```

   **Judge this by the outcome, not the exit code**, and re-run the check from step 1 to
   decide whether it worked.

   A current server understands `--install-only`, installs, records which service to talk
   to, and exits 0. An older one does not know the flag, ignores it, installs anyway, then
   reaches a step that needs a terminal, prints "No terminal available", and exits 3. The
   install succeeded in both cases. Treat a missing binary as the failure, never the exit
   code.

   If the binary is present but `rankme status` reports an `api` of `127.0.0.1`, that is the
   older path: tell the user to run `rankme` in their terminal, and skip to step 5.

3. **Show what would be read.**

   ```
   rankme discover
   ```

   This command has no prompt at all. Add `--all-repos` only if the user asked to scan
   the whole machine.

4. **Measure it, locally.**

   ```
   rankme analyze --yes
   ```

   `--yes` answers the *scope* confirmation, which is deliberately not the irreversible
   one — it means "I know what this reads", never "publish it". Nothing is uploaded.

   On a large corpus this takes a minute or two. Let it finish.

   **Show the source and evidence tables verbatim.** The counts and denominators are the
   whole point; a paraphrase throws away the thing the user came for.

5. **Hand over one line, and stop.**

   Tell them to run this in their own terminal:

   ```
   rankme
   ```

   Then say what it will do: re-scan, show exactly what would be sent, ask once, and
   publish only if they say yes. If `rankme` is not on their PATH, give them
   `~/.local/bin/rankme` instead.

## What you must not do

- **Never run `publish`.** Not with `--yes` — that flag cannot answer that prompt, by
  design. Not "just to see". Not because the user said to go ahead. If they insist, say
  you cannot answer a consent prompt on their behalf, and give them the line again.

- **Never manufacture a terminal.** No `script -q /dev/null`, no `expect`, no
  `unbuffer`, no `socat`, no `python -c 'import pty'`, no `yes |`.

  This is written down because it is the clever idea somebody will have, and it *would*
  work — a pty makes `/dev/tty` openable. That is exactly why it is forbidden. A
  terminal you create is a terminal whose user is **you**, and the "y" typed into it
  would be yours, not theirs. The prompt exists so that a person decides whether their
  work goes onto a public website under their name. Silence is never consent, and an
  answer nobody gave is worse than silence: it leaves a record of a decision that was
  never made.

- **Never read or echo anything from `~/.builder`.** It holds a device token and a key.
  Nothing there belongs in a transcript.
