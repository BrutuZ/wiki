---
title: pushover
description: 
published: true
date: 2022-09-18T05:26:40.051Z
tags: 
editor: markdown
dateCreated: 2022-09-18T05:26:37.408Z
---

# [Notifiers](/Plugins/Notifiers) > Pushover
<div class="alert alert-success" role="info">
  
  <span class="glyphicon glyphicon glyphicon-cog"></span>
  &nbsp; Pushover is a part of the [notifier](/Plugins/Notifiers) plugin system.
</div>



This plugin provides the ability to send flexget notifications via the cross-platform notification system called [Pushover](https://pushover.net/apps/clone/Flexget).

## Configuration

| Option |Type|  Description | Default |
| --- | ---| --- |---|
| **user_key**| text| Pushover's user key. **Required.** Can also be a list.
| api_key| text| Application's API key| Flexget's official key|
|device|text|Target a specific device. Can also be a list|
|url|URL|Notification URL | 
|priority|numeric| Set message priority. Values between -2 and 2 are accepted.| `0`|
|url_title|text|Text to be displayed as URL title 
|sound|text|Override default notifcation sound. Must one of [Pushover's supported sounds](https://pushover.net/api#sounds)
|retry|numeric|Number of seconds between notifications retries. Relevant only if priority is set to 2. Minimum is 30.
|expire|numeirc|Number of seconds to keep notification alive. Relevant only if priority is set to 2. Minimum value is 30. Maximum value is 86400.
|callback|URL|A callback URL to receive acknowledgement from notifications. Relevant only if priority is set to 2
|html|yes/no|Formats the messages with HTML tags. Requires Pushover device version to be 2.3 or higher

### Examples

Examples show a specifc scenario usage of the [notify](/Plugins/notify) plugin. See its wiki for a more detailed usage exaplantion.

#### Simple notification for each accepted entry
The simplest Pushover plugin configuration requires only the user key (`user_key`). This will broadcast the notification to all registered devices.

```yaml
notify:
  entries:
    via:
      - pushover:
          user_key: USER_KEY
```

#### More advanced version
```yaml
notify:
  entries:
    title: Downloading {{series_name}}
    message: Episode {{series_id}}
    via:
      - pushover:
          user_key: 
            - o23ywmAaaxTYxn00jY2JAwQ2EeYXGt    
            - 0ydnaF3023jKadfkja9fjdkjaXfdfsaySGa
          api_key: nqC2fSOLCEyHHJcnusQtw4wqG2WbWf
          device: mobile
          priority: 2
          url: http://server.example.com/path/to/downloader/ui
          sound: incoming
          retry: 60
          expire: 1000
```

#### Example with Jinja2 tags
```yaml
notify:
  entries:
    title: >
      {%if task in ["task_a","task_b"](/"task_a","task_b")%} New movie added to queue
      {%else%}Download Started from task {{task}}
      {%endif%}
    message: >
      {% if series_name is defined %}{{series_name}} - {{series_id}} - {{trakt_ep_name}} - {{quality|d('')}}
      {% elif imdb_name is defined%}{{movie_name}} - {{quality}}
      {% else %}{{title}}
      {% endif %}
    via:
      - pushover:
          user_key: '{? credentials.pushover.user_key ?}'
          api_key: '{? credentials.pushover.api_key ?}'
          sound: bike
          url: '{{trakt_series_trakt_url|d(imdb_url)}}'
          url_title: Link
          priority: >
            {% if task == "retreive_from_couchpotato" %}-1
            {% else %}0
            {% endif %}
```
#### Example with HTML tags
```yaml
notify:
  entries:
    via:
      - pushover:
          user_key: '{? credentials.pushover.userkey ?}'
          html: yes
    message: |+
      <b>word</b> - display word in bold
      <i>word</i> - display word in italics
      <u>word</u> - display word underlined
      <font color="blue">word</font> - display word in blue text (most colors and   hex codes permitted)
      <a href="http://example.com/">word</a> - display word as a tappable link to http://example.com/
```