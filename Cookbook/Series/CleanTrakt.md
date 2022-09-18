---
title: CleanTrakt
description: 
published: true
date: 2022-09-18T05:20:27.605Z
tags: 
editor: markdown
dateCreated: 2022-09-18T05:20:25.092Z
---

# Cleaning a Trakt List
If you are using configure_series with a trakt list, periodically you may wish to remove shows which have ended. Nobody wants to do that manually, so here is a task to help.
The `list_add` is optional, but maybe you want to move the show to an *Archive* list after having it removed from your *Watching* list?
```
trakt_list:
  username: ***
  account: ***
  list: Watching #custom trakt list "Watching"
  type: shows
trakt_lookup: yes
disable: seen #is necessary to not get cross-contamination effects with caching
if:
  - trakt_collected and trakt_series_status in ['ended', 'canceled']: accept
list_add: #optional
  - trakt_list:
    username: ***
    account: ***
    list: Archive #custom trakt list "Archive"
    type: shows
list_remove:
  - trakt_list:
    username: ***
    account: ***
    list: Watching
```