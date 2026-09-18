# rankme

Rank yourself as a coder. Measured.

A Claude Code plugin that measures the AI coding work already on your machine and shows you
the evidence.

```
/plugin marketplace add jatinjain25/rankme
/plugin install rankme@paxel
/rankme
```

It reads your local Claude Code, Cursor, Codex and opencode sessions, measures them **on your
own machine**, and prints what it found. Nothing is uploaded by anything the plugin runs.

Publishing a score is a separate, deliberate step you take in your own terminal, because the
confirmation is read from the terminal and cannot be answered by a tool, a flag, or an agent.
The plugin will hand you the one line to run and will not run it for you.

## What this repository contains

The plugin: a marketplace manifest, a plugin manifest, and one command file. That is all it
will ever contain. No source code and no binaries are here, and none are going to appear here.

The collector it installs is distributed as signed binaries from a separate repository, and its
source is not public.

## Licence

Copyright (c) 2026 Jatin Jain. All rights reserved. See [LICENSE](LICENSE).
