---
name: mojulo
description: Build, edit and export deterministic 3D objects, walkable worlds and games by running Mojulo locally in the current coding-agent workspace. Use for procedural 3D modeling, cities, rooms, worlds, game levels, 3D-printable objects, or GLB, HTML, STL, 3MF and bundle exports.
---

# Mojulo

Run Mojulo locally in the active writable workspace. Do not send geometry, renders or exports to a hosted Mojulo service unless the user explicitly asks for a remote workflow.

## Bootstrap

1. Check whether `.mojulo-runtime/bin/mojulo-agent` exists in the active workspace.
2. If it does not, run `scripts/bootstrap.sh` from this skill with the workspace path as its first argument.
3. The bootstrap installs exactly `mojulo@3.0.0`.
4. Use the generated workspace-local launcher for every Mojulo command.
5. If Node is missing or older than 22.14, report the requirement instead of modifying the system globally.

## Operate Mojulo

Start each new Mojulo task with:

```sh
.mojulo-runtime/bin/mojulo-agent orient
```

Then use Mojulo's own routing and schemas:

- `.mojulo-runtime/bin/mojulo-agent tools`
- `.mojulo-runtime/bin/mojulo-agent packs`
- `.mojulo-runtime/bin/mojulo-agent help <tool-or-pack>`
- `.mojulo-runtime/bin/mojulo-agent call <tool> --json '<object>'`
- `.mojulo-runtime/bin/mojulo-agent <pack> <tool> --json '<object>'`

Follow Mojulo's returned orientation and help instead of inventing arguments.

When editing an existing artifact, preserve its returned ref and mutate the same stored recipe rather than silently creating a replacement.

## Artifacts and recovery

Mojulo state belongs under `.mojulo` in the active workspace unless Mojulo returns another workspace-local path.

Return actual produced artifacts to the user through the host agent's normal file handoff.

For portability or recovery tests, save the recipe/checkpoint, restore it in a fresh workspace, and compare hashes when deterministic verification is requested.

## Failure behavior

Never claim an export, restore, render or deterministic match unless the corresponding Mojulo command succeeds and the expected output exists.
