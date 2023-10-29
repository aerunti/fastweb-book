# install

add repository

```
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
wget -qO- https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo tee /etc/apt/trusted.gpg.d/pgdg.asc &>/dev/null
apt update
apt install postgresql postgresql-client -y
```

create a new database and a superuser 

```
sudo -u postgres psql -c 'create database newdatabase;'
sudo -u postgres psql -c "create role newuser login superuser 'newpassword';"
```

remote access

edit /etc/postgresql/15/main/postgresql.conf
```
listen_addresses = '*' #'localhost
```

edit /etc/postgresql/15/main/pg_hba.conf
```
# IPv4 local connections:
host    all             all             127.0.0.1/32            scram-sha-256
# IPv4 remote connections:
host    all             all             0.0.0.0/0               scram-sha-256
```

restart postgreql server
```
service postgresql restart
service postgresql status
```
