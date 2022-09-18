---
title: npo_watchlist
description: 
published: true
date: 2022-09-18T05:09:06.148Z
tags: 
editor: markdown
dateCreated: 2022-09-18T05:09:03.529Z
---

# NPO watchlist
Produces entries for every episode on the user's npo.nl watchlist (Dutch public television). To add series / episodes, login to npo.nl and add them to your profile.

Entries can be downloaded using [download-npo](https://github.com/Carpetsmoker/download-npo).


| **Option** | **Description** |
| --- | --- |
| **email** | Your username for npo.nl |
| **password** | Your password for npo.nl |
| **remove_accepted** | If set to 'yes', the plugin will delete accepted entries from the watchlist after download is complete. Defaults to 'no'. |
| **max_episode_age_days** | If set (and not 0), entries will only be generated for episodes broadcast in the last x days. This only applies to episodes related to series the user is following.
| **download_premium** | If 'download_premium' is set to 'yes', the plugin will also download entries that are marked as exclusive content for NPO Plus subscribers.

## Example
```
your_task:
  npo_watchlist:
    email: aaaa@bbb.nl
    password: xxx
    remove_accepted: yes
    max_episode_age_days: 7
    download_premium: no
  accept_all: yes
  exec:
    fail_entries: yes
    auto_escape: yes
    on_output:
      for_accepted:
        - download-npo -o "path/to/directory/{{series_name_plain}}" -f "{serie_titel} - {datum} {aflevering_titel} ({episode_id})" -t {{url}}