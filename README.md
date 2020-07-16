# deployment

A repository with scripts, docker-compose, etc to guide deployment.

## How

This deployment repo contains a single docker-compose file deployed with docker swarm. The secrets are injected using:

    printf "strava-client-secret-here" | docker secret create strava_client_secret -

There are currently 3 secrets defined:
- strava_client_id: The strava client id.
- strava_client_secret: The associated secret.
- db_connection_string: The connections string to the postgres db.

Continuous deployment is setup using a simple cronjob:

    */1 * * * * cd /var/deployment && git pull && docker stack deploy -c docker-compose.yml api --with-registry-auth
