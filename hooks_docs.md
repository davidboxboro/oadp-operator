# Velero Hooks 

## Overview

Velero hooks enable the execution of terminal commands before and after resources are backed up. 

Before a backup, "pre" hooks are used to freeze resources so that they cannot be modified as the backup is taking place.
After a backup, "post" hooks are used to unfreeze those resources so that can return to their normal state afte the backup is complete.

## Postgres (Patroni)

The Velero hooks are specified as annotations in the [template](https://github.com/devarshshah15/velero-examples/blob/debug/patroni/template_patroni_persistent.yaml#L108).

These lines specify the "pre" hook to freeze resources:

```
pre.hook.backup.velero.io/command: '["/bin/bash", "-c","patronictl pause && pg_ctl stop -D pgdata/pgroot/data"]'
pre.hook.backup.velero.io/container: patroni-persistent
```

These lines specify the "post" hook to unfreeze them:

```
post.hook.backup.velero.io/command: '["/bin/bash", "-c", "patronictl resume"]'
post.hook.backup.velero.io/container: patroni-persistent 
```

## Cassandra

The Velero hooks are specified as annotations in the [template](https://github.com/kurwang/velero-examples/blob/cassandra-branch/cassandra/roles/install-cassandra/templates/cassandra-app.yaml.j2#L18).

This line specifies the "pre" hook to freeze resources before backup:

```
pre.hook.backup.velero.io/command: '["/bin/bash", "-c", "opt/cassandra/bin/nodetool drain"]'
```

