```
Scenario: "Manhattan": can't write data into database.

Level: Medium

Description: Your objective is to be able to insert a row in an existing Postgres database. The issue is not specific to Postgres and you don't need to know details about it (although it may help).

Helpful Postgres information: it's a service that listens to a port (:5432) and writes to disk in a data directory, the location of which is defined in the data_directory parameter of the configuration file /etc/postgresql/14/main/postgresql.conf. In our case Postgres is managed by systemd as a unit with name postgresql.

Test: (from default admin user) sudo -u postgres psql -c "insert into persons(name) values ('jane smith');" -d dt

Should return:INSERT 0 1

Time to Solve: 20 minutes.

OS: Debian 10
```

## Solving process
1. Check test.
```bash
$ sudo systemctl status postgresql
$ ps aux | grep postgres
```

2. try
```bash
$ sudo systemctl start postgresql
```

3. check err
```bash
$ journalctl -p err
$ grep postgres /var/log/syslog
```

4. check disk space
```bash
$ df -h
$ grep -i 'no space left' /var/log/syslog
$ cd opt/pgdata | ls -l
$ du -sh /opt/pgdata/{each_file}
```

5. Delete heaviest file
```bash
$ rm opt/pgdata/*.bk
$ sudo systemctl start postgresql
```

