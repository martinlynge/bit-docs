---
id: cli-status
title: Status
permalink: docs/cli-status.html
layout: docs
category: CLI Reference
prev: cli-show.html
next: cli-tag.html
---

Displays the status of all the components currently under work.
Meaning - displays new, modified and staged components.
Does not display components that have already been exported, and components that have been imported but not modified.

## Synopsis

```bash
bit status|s
```

## Examples

```bash
bit status
```

Output will be:

```bash
new components
     > foo/bar... ok


no modified components


staged components
     > moon/sun... ok
```

### Component status definitions

* **New** - a newly-created component, before first tagging.
* **Modified** - a component that has been modified and isn't new (previously staged/exported/imported).
* **Staged** - a component that had been tagged has remained unmodified. Not yet exported.