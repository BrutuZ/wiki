---
title: email
description: 
published: true
date: 2023-01-27T22:49:13.071Z
tags: 
editor: markdown
dateCreated: 2022-09-18T05:25:38.605Z
---

# [Notifiers](/Plugins/Notifiers) > Email

> Email is a part of the [notifier](/Plugins/Notifiers) plugin system.
{.is-success}

The email plugin can be used to notify you of task results and/or failures. There are two built in templates, or you can make your own template using the Jinja2 templating language.

The `default` template will notify you of all downloaded entries, and of any failed entries or task aborts. There is also an included `failed` template to just notify when there are problems with a task.

## Config

| Options |Type|  Description | Default |
| --- | ---| --- |---|
| to | email| The email address(es) of the recipient(s). **Required.**
| from| email| The email address from which the email will be sent. |`flexget_notifer@flexget.com` | 
| smtp_host | text|The host of the smtp server. |`localhost`| 
| smtp_port |numeric| The port of the smtp server. | `25`| 
| smtp_username |text| The username to use to connect to the smtp server| 
| smtp_password |text| The password to use to connect to the smtp server| 
| smtp_tls |yes/no| Should TLS be used to connect | no|
| smtp_ssl | yes/no|Should SSL be used to connect| no
|html| yes/no | Should parse message as HTML|no
|autofrom| yes/no | Automatically Generate From address based on username and system hostname|no




### Built-In Templates

- `default`: This will send emails with a list of accepted entries, and/or a list of failed entries. (this template is used automatically if you do not specify one.)  
- `html`: This attempts to make html formatted emails with images for series and movies.
- `rss`

<b>Note: When using html template, use `tvdb_lookup: yes` in your task, to make sure metainfo is filled in.</b>

### Custom Templates
You can create your own custom templates for the email plugin in the jinja2 templating language. They should be placed in `/templates` in config path, and their filename specified as the `template` option. See the [default template](https://github.com/Flexget/Flexget/blob/develop/flexget/templates/task/default.template) for an example.

### Examples
> Examples show a specifc scenario usage of the [notify](/Plugins/notify) plugin. See its wiki for a more detailed usage exaplantion.
{.is-warning}

**Config basic example**

```yaml
notify:
  task:
    template: html  # Optional, if you want html instead of plain text
    via:
      - email:
          from: xxx@xxx.xxx
          to: xxx@xxx.xxx
          smtp_host: smtp.host.com
          html: yes # To parse template as HTML
```

**Config example with smtp login and multiple recipients**

```yaml
notify:
  task:
    template: html  # Optional, if you want html instead of plain text
    via:
      - email:
          from: xxx@xxx.xxx
          to:
            - xxx@xxx.xxx
            - yyy@yyy.yyy
          smtp_host: smtp.host.com
          smtp_port: 25
          smtp_username: my_smtp_login
          smtp_password: my_smtp_password
          html: yes # To parse template as HTML
```
**Gmail example**
```yaml
notify:
  task:
    template: html  # Optional, if you want html instead of plain text
    via:
      - email:
          from: from@gmail.com
          to: to@gmail.com
          smtp_host: smtp.gmail.com
          smtp_port: 587
          smtp_username: gmailUser
          smtp_password: gmailPassword
          smtp_tls: yes
          html: yes # To parse template as HTML
```
**Config example - Autofrom option for systems with local MTA**
```yaml
notify:
  task:
    via:
      - email:
          autofrom: yes
          to: xxx@xxx.xxx
```

