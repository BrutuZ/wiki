---
title: subtitle_list
description: 
published: true
date: 2022-09-18T05:25:25.343Z
tags: 
editor: markdown
dateCreated: 2022-09-18T05:25:22.645Z
---

# Subtitle list
> This is part of [managed list](/Plugins/List) plugin system.
{.is-success}

<div class="alert alert-info" role="alert">
  <span class="glyphicon glyphicon glyphicon-download-alt"></span>
  &nbsp;
This plugin requires the <a href="http://subliminal.readthedocs.io/">subliminal</a> Python library, version 2.0 or newer. To install it, run the following command:
<br/><br/>

```cmd
pip install subliminal>=2.0
```
</div>

The main purpose of this plugin is to keep track of local video files that are in need of subtitles. While it allows directories (if specified in config), it's a bit finicky. If you experience any issues, be it unexpected behaviour or bugs, report it on our Github page (http://github.com/Flexget/Flexget).
  

**Notes:** 

 * This plugin should be used together with [subliminal](/Plugins/subliminal) plugin.
 * When outputting a directory from the list, it will attempt to search for video files with the specified recursion depth (default 1 ie. no recursion) and add them to the list. If it finds any video files it will delete the dir from the list. Thus it's your own responsibility to make sure that *all* or *none* of the files are present when a task with [list_match](/Plugins/List/list_match) is executed.

## Plugin Settings
Currently the following settings are supported:

|  Option  |  Description  |
| --- | --- |
| **list** | User specified name of the list eg. "ru subtitles" |
| **languages** | A list of IETF language codes eg. 'en', 'ru' (a complete list: http://data.okfn.org/data/core/language-codes#resource-language-codes) |
| **check_subtitles** | Check for existing subtitles be it internal or external (requires subliminal >=2.0rc1). |
| **remove_after** | The interval after which the file should be removed from the list -- format: [n][minute&#124;hours&#124;days&#124;months] eg. "7 days". |
| **path** | The path to save in the list. Can either be a relative or full path. Supports jinja replacement. If it's not set, it will attempt to use the entry's location field. See below for example usage. |
| **allow_dir** | Whether to allow the path to be a directory when adding to the list. |
| **force_file_existence** | Check for file existence when adding to list and using the list as input. Removes non-existent files/dirs from the list if "yes". |
| **recursion_depth** | Parameter specifying how deep it should search for files when outputting from the list. Setting it to 1 means it won't search in any subfolders. |

## Examples

### Add video files while moving
This example shows how you would use subtitle_list in a task that sorts episodes based on their series_name.

```yaml
move_shows:
  metainfo_series: yes 
  filesystem:
    path:
      - ~/Downloads
    regexp: '.*\.(avi|mkv|mp4)$'
    recursive: yes
    retrieve: files
  thetvdb_lookup: yes
  accept_all: yes
  move:
    to: /some/path/{{ series_name }}
  list_add:
    - subtitle_list:
        list: subtitles
        languages:
          - en
```

### Download subtitles
This example shows how to use the files in a subtitle list along with [subliminal](/Plugins/subliminal) to download subtitles.

```yaml
get_subs:
  no_entries_ok: yes
  subtitle_list:
    list: subtitles
    recursion_depth: 3
  list_match:
    from:
      - subtitle_list:
          list: subtitles
  subliminal:
    languages:  # languages is required, but if a language is specified in subtitle_list, it takes priority
      - en
    providers: 
      - addic7ed
      - opensubtitles
      - podnapisi
      - tvsubtitles
    single: no  # will append the language code to the subtitle file
    exact_match: no
```