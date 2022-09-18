---
title: Upgrade
description: 
published: true
date: 2022-09-18T05:01:35.090Z
tags: 
editor: markdown
dateCreated: 2022-09-18T05:01:32.483Z
---

# Upgrade

The upgrade plugin will continue getting better qualities of an entry (tracked by a unique identifier). It can also reject entries that are of lower quality compared to the latest accepted quality.

## Settings

| **Option** | **Description** |
| --- | --- |
| identified_by | Define how entries are identified, default `auto` which uses entry id [field](https://flexget.com/Entry). Supports [Jinja Template](https://flexget.com/Jinja) |
| tracking | If enabled by it's self will track entry but not upgrade. |
| target | Continue to upgrade until reaching target quality OR better.NOTE: Use the [quality](https://flexget.com/Plugins/quality) plugin to restrict the uppper quality. |
| on_lower | The action to preform on entries which are lower then the best or existing quality. `do_nothing` won't act on the entry ([undecided](https://flexget.com/FilterOperations))  but allow it to by accepted by other plugins |
| timeframe | Allow upgrades for the given peroid of time. NOTE: This options takes priority |
| propers | Allow upgrade to propers. NOTE: If timeframe has reached propers will be rejected |


## Syntax:

```text
upgrade:
  identified_by: <jinja template>
  tracking: [yes|no]
  target: <quality requirement>
  on_lower: [accept|reject|do_nothing]
  timeframe: <NUM (minutes|hours|days|weeks)>
  propers: [yes|no]
```

### Example for movies
In this example, the first task will download movies based on imdb ratings. The second task will upgrade the movies already downloaded for 7 days.

```yaml
tasks:
  high_rated_movies:
    upgrade:
      # We must add this so the upgrade plugin can track the downloaded qualities
      tracking: yes
    imdb_high_rated:
      imdb:
        min_score: 8.5
        min_votes: 5000

  upgrade_movies:
    upgrade:
      target: <=1080p
      propers: yes
```

### Example for series

Series currently has the concept of an upgrade. In the future series will be migrated to leverage this upgrade plugin.

In this example, the first task will download episodes for existing series on the filesystem. The second task will upgrade any downloaded series.

```yaml
tasks:
  existing_tv_shows:
    upgrade:
      # We must add this so the upgrade plugin can track the downloaded qualities
      tracking: yes
    configure_series:
      from:
        filesystem:
          - /media/series/

  upgrade_tv:
    upgrade:
      target: <=1080p
      propers: yes
```

### Example with custom identifier
The [metainfo_series](https://flexget.com/Plugins/metainfo_series) and [metainfo_movie](https://flexget.com/Plugins/metainfo_movie) plugins will, by default, set an identifier.

You can override the format using [Jinja Templates](https://flexget.com/Jinja).

```yaml
tasks:
  some_task:
    upgrade:
      identifier: "{{ some_field }}"
      tracking: yes
      target: 1080p
```

### Example with [timeframe](https://flexget.com/Plugins/timeframe)

In this example, the first task will download episodes for existing series on the filesystem. It will wait for 720p for 6 hours if not accept whatever quality is available.

The second task will upgrade any downloaded series.

```yaml
tasks:
  existing_tv_shows:
    upgrade:
      # We must add this so the upgrade plugin can track the downloaded qualities
      tracking: yes
    timeframe:
      wait: 6 hours
      target: 720p+
    configure_series:
      from:
        filesystem:
          - /media/series/

  upgrade_tv:
    upgrade:
      target: <=1080p
      propers: yes
```



