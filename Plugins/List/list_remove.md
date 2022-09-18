---
title: list_remove
description: 
published: true
date: 2022-09-18T05:25:01.587Z
tags: 
editor: markdown
dateCreated: 2022-09-18T05:24:58.968Z
---

## List Remove
<div class="alert alert-success" role="info">
  
  <span class="glyphicon glyphicon glyphicon-cog"></span>
  &nbsp; This is part of [managed list](/Plugins/List) plugin system.
</div>

List plugins can have entries removed from them by using the `list_remove` plugin:
```
trakt_list:
  username: traktusername
  list: watchlist
  type: movies
accept_all: yes
list_remove:
  - trakt_list:
      username: a_different_username
      account: traktusername # required if list is not public
      list: watchlist
      type: movies
```