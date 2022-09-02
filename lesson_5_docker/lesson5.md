# Lesson 5 - Docker

### How to get help ###

List of all commands:  

```
docker --help
``` 

Help on one command:

```
docker <COMMAND> --help
``` 

Docker Commands References:  https://docs.docker.com/engine/reference/commandline/cli/


### Example: run PostgreSQL in a container ###

1. Get docker disk usage

```
docker system df
```

2. Install psql

```
yay -S postgresql-libs
```

3. Pull PostgreSQL image from Dockerhub

```
docker pull postgres:14.5-alpine3.16
```

4. Get list of images

```
docker image list
```

Image `postgres:14.5-alpine3.16` is in the list


5. Make empty directory `pgdata` for databases storage

```
pwd
```

Output: `/home/andrey`

```
mkdir ./pgdata
```

Directory `/home/andrey/pgdata` is created

6. Create and run PostgreSQL container with database directory mounted in the host file system, initially create database `demodb` 

```
docker run \
	--name demo-postgres \
	-e POSTGRES_USER=andrey \
	-e POSTGRES_PASSWORD=demopassword \
	-e PGDATA=/var/lib/postgresql/data \
	-e POSTGRES_DB=demodb \
	-p 6432:5432 \
	--mount 'type=bind,source=/home/andrey/pgdata,target=/var/lib/postgresql/data' \
	-d \
	postgres:14.5-alpine3.16
```

Output: `<CONTAINER_ID>` (Container is created and running)


7. Get list of all containers

```
docker ps -a
```

There is `demo-postgres` container with Status `Up`


8. Get container logs

```
docker logs <initial_part_of_CONTAINER_ID>
```
Log records with initialization part are displayed


9. Connect to `demodb` database from the host (VM Manjaro)

```
psql -h 127.0.0.1 -p 6432 -d demodb
```

`Type password according to POSTGRES_PASSWORD on Step 6.`

Prompt changed to `demodb=#`


10. Get list of tables

```
\dt
```

The list is empty, no table is in the database	


11. Open the command editor 

```
\e
```

Vi editor opened

```
Hit <Ins>
```

Editor switched to Insert mode


12. Paste the command

```
CREATE TABLE accounts (
        user_id serial PRIMARY KEY,
        username VARCHAR ( 50 ) UNIQUE NOT NULL,
        password VARCHAR ( 50 ) NOT NULL,
        email VARCHAR ( 255 ) UNIQUE NOT NULL,
        last_login TIMESTAMP
);
```


13. Save and run the command

```
Hit <Esc>
Type ':wq!' (without quotes)
Hit <Enter>
```

Only `CREATE TABLE` from the command is seen, but the table has been created


14. Get list of tables in `demodb` database

```
\dt
```

One record: table `accounts`


15. Quit psql

```
\q
```

Prompt changed back to VM Manjaro prompt


16. Run Bash terminal inside the container

```
docker exec -it demo-postgres /bin/bash
```

Prompt changed to container's OS prompt `bash-5.1#`
	

17. Get the current user login

```
whoami
```
Output: `root`	


18. Connect to `demodb` database from inside the container

```
psql -h 127.0.0.1 -p 5432 -d demodb -U andrey
```

Prompt changed to `demodb=#`


19. Get list of tables in `demodb` database

```
\dt
```

One record: table `accounts`


20. Insert records to `accounts` table

```
INSERT INTO accounts(username, password, email)  VALUES ('admin', '123', 'adimn@here.com');
INSERT INTO accounts(username, password, email)  VALUES ('user', '#dRe/^', 'user@here.com');
```


21. Get all records from `accounts` table

```
SELECT * FROM accounts;
```

Two records are displayed that have been inserted on the previous step	


22. Quit psql

```
\q
```

Prompt changed back to container's OS prompt `bash-5.1#`


23. Quit the container

```
exit
```

Prompt changed to VM Manjaro prompt	


24. Stop the container `demo-postgres`

```
docker stop demo-postgres
```

Output: `demo-postgres`	(Container is stopped)


25. Get list of all containers

```
docker ps -a
```

Status of `demo-postgres` container is 'Exited'


26. Get list of running containers

```
docker ps
```

No running container `demo-postgres`


27. Remove the container

```
docker rm <initial_part_of_CONTAINER_ID>
```

Output: `<initial_part_of_CONTAINER_ID>` (Container is removed)


28. Get list of all containers

```
docker ps -a
```

No container `demo-postgres`


29. Start a new PostgreSQL container to check that the data is persisting

```
docker run \
	--name demo-postgres-check \
	-e POSTGRES_USER=andrey \
	-e POSTGRES_PASSWORD=demopassword \
	-e POSTGRES_DB=demodb \
	-e PGDATA=/var/lib/postgresql/data \
	-p 7432:5432 \
	--mount 'type=bind,source=/home/andrey/pgdata,target=/var/lib/postgresql/data' \
	-d \
	postgres:14.5-alpine3.16
```

Output: `<CONTAINER_ID>` (Container is created and running)


30. Get container logs

```
docker logs <initial_part_of_CONTAINER_ID>
```

Log records are displayed. Initialization is skipped because the database directory contains data


31. Connect to the database from the host (VM Manjaro)

```
psql -h 127.0.0.1 -p 7432 -d demodb
```

`Type password according to POSTGRES_PASSWORD on Step 29.`

Prompt changed to `demodb=#`


32. Get list of tables in `demodb` database

```
\dt
```

One record: table `accounts`


33. Get all records from `accounts` table

```
SELECT * FROM accounts;
```

Two records are displayed that have been inserted on the Step 20


SUCCESS


33a. Quit psql

```
\q
```

34. Stop the running container

```
docker stop demo-postgres-check
```

35. Remove the stopped container

```
docker rm demo-postgres-check
```

36. Clean the system (even if we skip step35 - this command will remove stopped container and image)

```
docker system prune -af
```
  

37. Get docker disk usage

```
docker system df
```
