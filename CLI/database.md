---
title: database
description: 
published: true
date: 2022-09-18T04:53:33.999Z
tags: 
editor: markdown
dateCreated: 2022-09-18T04:53:31.399Z
---

## [CLI](/CLI) > `database`
Utilities to manage the FlexGet database.

### Sub-commands
| Sub-command | Option | Description |
| --- | --- | --- |
| `cleanup` || Cause all plugins to clean unnecessary data from the database |
| `vacuum` || Optimize the database, possibly increasing performance and decreasing database size |
| `reset` || Resets the entire database (DANGEROUS!) |
|| `--sure` | Required with `--reset` to confirm you want to really reset the database.|
| `reset-plugin` || Reset the database for a specific plugin |
|| `<plugin name>` | Name of the plugin to reset |

### Examples
```bash
# cleanup unnecessary data from the database
$ flexget database cleanup
```