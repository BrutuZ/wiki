---
title: per_series_settings
description: 
published: true
date: 2022-09-18T05:28:19.107Z
tags: 
editor: markdown
dateCreated: 2022-09-18T05:27:59.687Z
---

## Per series settings
When specifying a setting for a series, you must add a colon to the end of the series name, and add 4 more spaces before the setting name:

```
series:
  - series name:
      [setting]: [value]
```

For example, with [quality](/Plugins/series/quality) settings:

```
series:
  - Series 1
  - Series 2:
      quality: hdtv
  - Series 3:
      quality: 720p
```