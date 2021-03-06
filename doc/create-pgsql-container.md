# create pgsql container

```sh
docker volume create pgdata
docker run -e POSTGRES_PASSWORD=`cat ~/security/postgrespwdfile` --name postgres --mount source=pgdata,target=/var/lib/postgresql/data -d -p 5432:5432/tcp postgres:11.5
```

data will persist between through volume ( locally mapped to `/var/lib/docker/volumes/pgdata/_data` )

setup your `~/.pgpass` to quick access

```
*:*:*:postgres:secret
```

the format is `hostname:port:database:username:password`

```sh
chmod 600 ~/.pgpass
```

and test

```sh
psql -h localhost -U postgres
```
