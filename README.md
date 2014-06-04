# Installation

## On the primary server

 - Install *postgresql_secure_wal_transfer* in /usr/local/bin

 - Authorize the slave server to run *postgresql_secure_wal_transfer* over ssh :
```
(
  echo -n 'command="/usr/local/bin/postgresql_secure_wal_transfer",from="1.2.3.4",no-agent-forwarding,no-port-forwarding,no-X11-forwarding,no-pty '
  cat ~postgres/.ssh/id_rsa.pub
) > ~postgres/.ssh/authorized_keys
```

## On the standby server

 - Install *wal_restore* in /usr/local/bin

 - Test WAL transfer :

```
echo -e "9.2\nmain\n00000001000000050000001E" \
| sudo -u postgres ssh -T $primary > /tmp/1.gz
```

 - Add the following to /var/lib/postgresql/9.2/standby/recovery.conf :

```
primary_conninfo = 'host=myhost user=myuser'
restore_command = 'wal_restore %f %p 9.2 main postgres@myhost'
standby_mode = on
```
