First, create a directory in your home directory for MSF files. You also need a directory to keep Postgres data, let’s keep it in the same place with MSF files.

```
mkdir $HOME/.msf4
mkdir $HOME/.msf4/database
```

Network

You need a docker network to assign a fixed IP to each container. Let’s create a network with a subnet of 172.18.0.0/16 and we call it msf.

```
docker network create --subnet=172.18.0.0/16 msf
```

Database

Now we need the database, here we are going to use postgres 11 with alpine based os. Let’s assign it to network msf and give IP 172.18.0.2, you need to mount a volume to keep the data, you also need to assign value to postgres’s username, password and database name.

```
docker run --ip 172.18.0.2 --network msf --rm --name postgres -v "${HOME}/.msf4/database:/var/lib/postgresql/data" -e POSTGRES_PASSWORD=postgres -e POSTGRES_USER=postgres -e POSTGRES_DB=msf -d postgres:11-alpine
```

MSF

Now we can run MSF on docker, but for the first time, we need to set database URL (including username, password, and database name in url). You also need to mount the volume to save the data. Lastly, you need to map the range of ports that MSF is going to use.

```
docker run --rm -it --network msf --name msf --ip 172.18.0.3 -e DATABASE_URL='postgres://postgres:postgres@172.18.0.2:5432/msf' -v "${HOME}/.msf4:/home/msf/.msf4" -p 8443-8500:8443-8500 metasploitframework/metasploit-framework
```

Save database setting

You can save database setting in MSF. To do so, inside MSF console execute db_save. Now you can run MSF docker without setting database URL.

```
docker run --rm -it -u 0 --network msf --name msf --ip 172.18.0.3 -v "${HOME}/.msf4:/home/msf/.msf4" -p 8443-8500:8443-8500 metasploitframework/metasploit-framework
```

MSF function in .bashrc (or .bash_profile on Mac OS)

```
function msf-docker() {
   if [ -z "$(docker network ls | grep -w msf)" ];
   then
       docker network create --subnet=172.18.0.0/16 msf
   fi
   if [ -z "$(docker ps -a | grep -w postgres)" ];
   then
       docker run --ip 172.18.0.2 --network msf --rm --name postgres -v "${HOME}/.msf4/database:/var/lib/postgresql/data" -e POSTGRES_PASSWORD=postgres -e POSTGRES_USER=postgres -e POSTGRES_DB=msf -d postgres:11-alpine
   fi
   docker run --rm -it --network msf --name msf --ip 172.18.0.3 -e DATABASE_URL='postgres://postgres:postgres@172.18.0.2:5432/msf' -v "${HOME}/.msf4:/home/msf/.msf4" -v "${HOME}/opt/metasploit/wordlists:/home/msf/wordlists" -v "${HOME}/opt/SecLists:/home/msf/SecLists" -v "$( pwd ):/home/msf/data" -p 8443-8500:8443-8500 metasploitframework/metasploit-framework
 }
 ```
 
