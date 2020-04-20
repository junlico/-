# Uninstallation

## Ubuntu 18.04

```bash
sudo apt-get --purge remove postgresql
sudo apt-get purge postgresql*
sudo apt-get --purge remove postgresql postgresql-doc postgresql-common
sudo apt autoremove
```

### Grep for all PostgreSQL packages

```bash
dpkg -l | grep postgres
```

```bash
# rc pgdg-keyring 2018.2 all keyring for apt.postgresql.org
sudo apt-get --purge remove pgdg-keyring
```

### Remove all of the PostgreSQL data and directories

```bash
sudo rm -rf /var/lib/postgresql
sudo rm -rf /var/log/postgresql
sudo rm -rf /etc/postgresql
sudo rm -rf /etc/apt/sources.list.d/pgdg.list
```
