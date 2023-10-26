# VM1 
## sudo nano /etc/hosts
## 10.15.0.181 postgres01 
## 10.15.0.182 postgres02 
## 10.15.0.183 postgres03

## sudo ufw allow OpenSSH
## sudo ufw allow from 10.15.0.0/24 to any port 5432
## sudo ufw enable
## sudo ufw status

# Install PostgreSQL for 3 server
## sudo apt-get install wget gnupg2 lsb-release curl apt-transport-https ca-certificates -y
## curl https://www.postgresql.org/media/keys/ACCC4CF8.asc | gpg --dearmor | sudo tee /usr/share/keyrings/pgdg.gpg > /dev/null 2>&1 
## sudo sh -c 'echo "deb [signed-by=/usr/share/keyrings/pgdg.gpg] http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
## sudo apt update
## sudo apt install postgresql-15
## sudo systemctl is-enabled postgresql
## sudo systemctl status postgresql
## sudo apt-get install postgresql-plperl-15 -y

# VM1 
## cd /var/lib/postgresql
## sudo -u postgres psql
### CREATE USER bucardo WITH SUPERUSER;
### CREATE DATABASE bucardo OWNER bucardo;
### CREATE DATABASE testdb;
### \c testdb;
### CREATE TABLE users (
###   id SERIAL PRIMARY KEY,
### first_name VARCHAR(255),
###   last_name VARCHAR(255) NOT NULL,
###   city VARCHAR(255)
### );

# for each server Ubuntu VM:
## sudo nano /etc/postgresql/15/main/postgresql.conf
# VM1
### listen_addresses = 'localhost, 10.15.0.181'
# VM2
### listen_addresses = 'localhost, 10.15.0.182'
# VM3
### listen_addresses = 'localhost, 10.15.0.183'

# VM1
## sudo nano /etc/postgresql/15/main/pg_hba.conf
Local connection and bucardo user
### local    all             all                                    trust
### local    all             bucardo                                trust

# Bucardo user remote connections
### host    all             postgres         10.15.0.183/24         trust
### host    all             bucardo          10.15.0.183/24         trust
### host    all             postgres         10.15.0.182/24         trust
### host    all             bucardo          10.15.0.182/24         trust
similar with all VM remain

# for all VM
## sudo systemctl restart postgresql
## ss -tulpn | grep postgres # check port 5432

# VM1
## sudo -u postgres psql
## sudo -u postgres psql -U bucardo -h 10.15.0.182
## sudo -u postgres psql -U bucardo -h 10.15.0.183
### similar with all VM remain

# VM1
## sudo apt-get install make libdbix-safe-perl libboolean-perl libdbd-mock-perl libdbd-pg-perl libanyevent-dbd-pg-perl libpg-hstore-perl libpgobject-perl libpod-parser-perl libencode-locale-perl -y
## sudo wget -q https://bucardo.org/downloads/Bucardo-5.6.0.tar.gz
## tar xf Bucardo-5.6.0.tar.gz && cd Bucardo-*/
## perl Makefile.PL && sudo make install
## sudo mkdir -p /var/run/bucardo /var/log/bucardo && touch /var/log/bucardo/log.bucardo
## bucardo install
## bucardo add database server1 dbname=testdb
## bucardo add database server2 dbname=testdb host=10.15.0.182
## bucardo add database server3 dbname=testdb host=10.15.0.183

## bucardo add table public.users db=server1
## bucardo add table public.users db=server2
## bucardo add table public.users db=server3

## bucardo add all tables --her=testdbSrv1 db=server1
## bucardo add all tables --her=testdbSrv2 db=server2
## bucardo add all tables --her=testdbSrv3 db=server3

## bucardo list relgroup

## bucardo add sync testdbSrv1 relgroup=testdbSrv1 db=server1,server2,server3
## bucardo add sync testdbSrv2 relgroup=testdbSrv2 db=server2,server1,server3
## bucardo add sync testdbSrv3 relgroup=testdbSrv2 db=server3,server2,server1

## bucardo list sync
## bucardo restart sync
## bucardo status
