# Season packs

This option can be used if you want to accept season packs in addition to (or instead) of episodes. At this time, only `ep` [identified](/Plugins/series/identified_by) shows can accept season packs.

## Limitations:

- Season packs are currently supported only to `ep` [identified](/Plugins/series/identified_by) series, meaning that only these patterns will be recognizes as a season pack:
  - `Foo.S01.720p.HDTV-Flexget`
  - `Bar.1xALL.720p.HDTV-Flexget`
- Partial season packs are **not** supported at this time: `Foo.S01.Part-1.720p.HDTV-Flexget`
- Complete series packs are also not supported.

## Usage:

There are several modes of usage for this option. 

#### Basic usage:
```yaml
season_packs: yes
```
This will enable accepting season packs for any series that did not download ANY episodes for that particular season.  

#### Example:
```yaml
tasks:
  series_task:
    rss: bla
    series:
      - Foo:
        season_packs: yes
    download: '/shows/{{ series_name }}'
```

Let's say you have a show name `Foo` which you have downloaded an episode already for season `1`. Now your task encounters the following file:
`Foo.S01.720p.HDTB-Flexget`. 

Setting `season_packs: yes` means "accept season packs for this show as long as no more than **0** episodes have been downloaded for this season". The idea is not to accept a season pack for a season which you have already have started to track induvidual episodes for.  
An alternate way to acheive the **exact** same behaviour as this is to set the config like this:
```yaml
tasks:
  series_task:
    rss: bla
    series:
      - Foo:
        season_packs: 0
    download: '/shows/{{ series_name }}'
```
The number `0` after season packs refers to the number of episodes each season is allowed to previously download before accepting a season pack. 

#### Manually specifying threshold:
In case you want to manipulate the number of allowed episodes when accepting a season pack, you can specify it like so:
```yaml
season_packs: 10
```
This will allow downloading up to to 10 episodes per season (in any order) and also accepting a season pack of the same season.

#### Ignoring episode threshold:
In case you do not care about setting an episode threshold, and would like to always accept a season pack regardless of how many episdoes were already downloaded for that particular series, use the following option:
```yaml
season_packs: always
```

#### Accepting only season packs:
In case you want to accept **only** season pack for a series, use the following option:
```yaml
season_packs: only
```

### Advanced usage
You can fine tune both of the previous options of the config even further like this:
```yaml
season_packs:
  threshold: 5
  reject_eps: yes
```
## Important Note: 
In all cases and modes, once a season pack is accepted, that particular season will be marked as completed, **and no more season packs or episodes for that particular season would be accepted.**

### Developer notes:
- If you're creating/modifying an input plugin, and would like it to emit search string that'll match season pack format, add `season_pack_lookup = True` to the entry. That way the estimator used by [discover](/Plugins/discover) plugin know to lookup that entity as a season and not an episode.
- If you're creating/modifying a search plugin, `series` parser adds `season_pack = True` to entries that were parsed to be a season pack, so you could search for that in case the relevant search site has different functionality for season packs.

