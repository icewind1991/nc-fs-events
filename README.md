# Filesystem change events

## Event formatting

Change events are formatted using json, with the specific format depending on the type of change.

### Write events

Write events include both the creation of new files or folders and the modification of existing files.

```json
{
  "event": "modify",
  "path": "/changed/or/created/file",
  "time": "2019-05-13_10:58:35-0400"
}
```

### Move events

Move events include both renamed file or folders and files or folders that are moved to a different location.

```json
{
  "event": "move",
  "from": "/source/path/of/file",
  "to": "/target/path/of/file",
  "time": "2019-05-13_10:58:35-0400"
}
```
#
## Delete events

```json
{
  "event": "delete",
  "path": "/path/of/deleted/file",
  "time": "2019-05-13_10:58:35-0400"
}
```

It's not necessary to send events for files inside of a deleted folder, only an event for the topmost folder is required,
but sending extra events for files inside a deleted folders will not introduce unexpected behaviour.

## Publishing the event

Currently redis is the only supported message queue for publishing change events, more might be added in the future.

### Redis

A redis list is used to store the events, events can be published by pushing them ([`LPUSH`](https://redis.io/commands/lpush)) into the list
by default the list `nc-fs-events` should be used but an administrator should be able to configure this.
