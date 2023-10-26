# MASTER Node

## login postgres
sudo -u postgres psql

## sudo nano /etc/postgresql/16/main/postgresql.conf

listen_addresses= '10.15.0.181' #server’s IP address
wal_level = logical
wal_log_hints = on

## sudo nano /etc/postgresql/16/main/pg_hba.conf
host    all             postgres         10.15.0.182/32        trust
host    all             postgres         10.15.0.183/32        trust

## Restart PostgresSQL
sudo systemctl restart postgresql

## Tar file main in /var/lib/postgresql/16/main/
tar -czvf main.tar.gz /var/lib/postgresql/16/main/

## Send main.tar.gz to slave node
scp -i ~/admin.pem/var/lib/postgresql/16/main.tar.gz admin@10.15.0.182:~/
scp -i ~/admin.pem/var/lib/postgresql/16/main.tar.gz admin@10.15.0.183:~/

# SLAVE Nodes

## Stop PostgreSQL service
sudo systemctl stop postgresql

## remove folder main
sudo rm -rf /var/lib/postgresql/16/main/

## copy main.tar.gz to /var/lib/postgresql/16/main/
sudo su
cp ~/main.tar.gz /var/lib/postgresql/16/
rm -rf main/
tar -xzvf main.tar.gz

## Grant ownership of the data directory to the postgres user.
sudo chown postgres -R /var/lib/postgresql/16/main/

## Restart postgresql service
sudo systemctl restart postgresql
