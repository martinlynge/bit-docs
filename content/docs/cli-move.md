---
id: cli-move
title: Move
permalink: docs/cli-move.html
layout: docs
category: CLI Reference
---
Moves a file/directory that's part of a tracked component to a new location.
This command will update the [.bitmap file](/docs/initializing-bit.html#bitmap) accordingly.

## Synopsis

```bash
bit move|mv <from> <to>
```

## Examples

Move a file that's part of a tracked component.

```bash
bit move src/foo/bar/index.js src/components/new/location/new-file-name.js
```

Move a directory that's part of a tracked component.

```bash
bit move src/foo src/components/new/location/foo
```