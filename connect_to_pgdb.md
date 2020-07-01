# How to remotely connect postgresql database 

## Prepare

Installation in Ubuntu

```bash
sudo apt-get install wget ca-certificates
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" >> /etc/apt/sources.list.d/pgdg.list'
sudo apt-get update
sudo apt-get install postgresql postgresql-contrib
```

Right after successfully installing your postgresql database, go and use the following command to test the database. 

```bash
$ sudo --user=postgres psql
```

then use the following command to display all users
```sql
\du
```

Next, we need a new user for daily usage. the "user" is equivalent to "role". 

```sql
$ create role newuser SUPERUSER LOGIN PASSWORD 'your_password';
```
do not forget the semicolon ";" in the end. 

now use \q to quit

```sql
\q
```

and log into the database with new created __newuser__ .

```bash
$ psql -h localhost -d postgres -U newuser
```

## Update postgresql.conf file 

Use netstate command to show the current opening ports  
```bash
$ netstat -nlt
```
You will see the 5432 port is limited to localhost only. we need to change it to 0.0.0.0
```
Proto Recv-Q Send-Q Local Address           Foreign Address         State  
tcp        0      0 127.0.0.1:5432          0.0.0.0:*               LISTEN  
```

Find the configuration file 
```bash
$ find / -name "postgresql.conf"
```
it is very high chance, you will find the conf file here
```
/etc/postgresql/11/main/postgresql.conf
```
go use vim to open and edit it
```bash
$ sudo vim /etc/postgresql/11/main/postgresql.conf 
```
do remember use sudo, or you will open the file in readonly mode. Next go and find 
```
listen_addresses = 'localhost'
```
replace it with 
```
listen_addresses = '*'
```
save and quit vim. 

## Update pg_hba.conf file 

Use the same method to find and open pg_hba.conf. add the following to the very end of the file. 
```
host    all             all              0.0.0.0/0                       md5
host    all             all              ::/0                            md5
```
save and quit vim. 

## Almost done 
Now restart postgresql service and you should be able to connect the database server remotly. 

```bash
$ sudo /etc/init.d/postgresql restart
```

