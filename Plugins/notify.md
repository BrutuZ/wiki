# Notify
This plugin can be used to send messages about different events during task execution. These messages can be delivered by one (or more) of the [notification delivery plugins](/Plugins/Notifiers). There are 3 main categories of message this plugin can send:

- **Entries**

  Sends a notification for each accepted entry in a task. (It can alternatively be configred to send messages about rejected or failed entries if desired.)

- **Task**
 
  Sends a notification when the task completes with either accepted or failed entries.

- **Abort**

  Sends a notification when the task aborts.

## Configuration
```yaml
notify:
  entries:
    [title]: Notification title.
    [message]: Notification body.
    [template]: Specify message from a template on disk.
    [what]: accepted|rejected|failed|undecided
    via:
      - <notifier name>:
          <notifier config>
  task:
    [always_send] yes|no. If set to Yes it will always send notification, even when no entries were accepted to task. Default is No. This is recommended only for advanced usage as it can produce many potentially empty notifications.
    [title]: Notification title.
    [template]: Specify message from a template on disk.
    via:
      - <notifier name>:
          <notifier config>
  abort:
    [title]: Notification title.
    [message]: Notification body.
    via:
      - <notifier name>:
          <notifier config>
```

**Note:** Any templates referenced in the configuration must be stored in a `templates` folder within your configuration directory.

See [notification delivery plugins](/Plugins/Notifiers) for specific usage.