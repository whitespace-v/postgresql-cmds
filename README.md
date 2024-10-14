
# Getting started

### Create the file repository configuration:
```
sudo sh -c 'echo "deb https://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
```
### Import the repository signing key:
```
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
```
### Update the package lists:
```
sudo apt-get update
```

### Install the latest version of PostgreSQL.
#### If you want a specific version, use 'postgresql-12' or similar instead of 'postgresql':
```
sudo apt-get -y install postgresql
```
# Database creation

#### 1. Enter to super user
```
sudo su
```
#### 2. Enter to postgres user
```
su - postgres
```
#### 3. Enter the psql shell
```
psql
```
#### 4. Create role
```
create user @username with password 'password';
```
#### 5. Create database
```
create database @databasename;
```
#### 6. Give permissions to user on database
```
grant all privileges on database @databasename to @username;
```
# PSQL commands

### Exit the psql shell

```
\q
```
### Show all tables
```
\dt
```
### Enter to the database from psql user
```
psql -h localhost @database @username
```
### Update value in table
```
update @table set @column='@value' where @column=@value;
```
#### Example: 
```
update users set role='Admin' where id=1;
```
### Add column in table
```
alter table @table alter column @column type @type;
```
#### Example: 
```
alter table price_items add "categoryId" numeric(10,2);
```
### Remove column from table
```
alter table @table_name drop columm column_name
```

## Dump Database

### If you don't know password to postgres user

#### 1. Enter to postgres user
```
su - postgres
```
#### 2. Enter the shell
```
psql
```
#### 3. Change the password
```
ALTER USER postgres PASSWORD 'mynewpassword';
```
### Dump & Restore

#### 1. From the docker
```
docker exec postgres pg_dump -U whale -F t whalebase | gzip > dump_$(date +%d-%m-%Y).tar.gz
```
#### 3. Extract
```
gzip -dk dump_02-10-2024.tar.gz
```
#### 2. Restore
```
docker exec -i postgres pg_restore -U whale --verbose --clean --no-acl --if-exists -d whalebase < dump_02-10-2024.tar
```
