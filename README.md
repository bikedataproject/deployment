# deployment

A repository with scripts, docker-compose, etc to guide deployment.

## How does it work

This deployment repo contains a single docker-compose file deployed with docker swarm. The secrets are injected using:

```sh
printf "strava-client-secret-here" | docker secret create strava_client_secret -
```

There are currently 8 secrets defined:

- `strava_client_id`: The strava client id.
- `strava_client_secret`: The associated secret.
- `db_connection_string`: The connections string to the postgres db.
- `db_host`: The host of the database (endpoint)
- `db_port`: The port on which the database is listening
- `db_user`: User that has access to the database
- `db_password`: The password for the database
- `db_name`: The name of the used database

Continuous deployment is setup using a simple cronjob:

```sh
*/1 * * * * cd /var/deployment && git pull && docker stack deploy -c docker-compose.yml api --with-registry-auth
```

## Architecture

The bikedata project consists of various servers:

The frontend server is hosted at https://www.bikedataproject.org/ - a relatively lightweight server.
Another server, the `data`-server (aka the `bikedataprojectdata`-server) does the heavy lifting (generating the tiles, keeping track of all the data, ...)
The webserver acts as a proxy for the data server, the webserver's NGINX forwards the necessary requests.
