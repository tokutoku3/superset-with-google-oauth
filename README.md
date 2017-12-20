# Superset

Docker image for [Superset](https://github.com/ApacheInfra/superset).

## Requirements

The following settings are required in the `.env` .
```
SUPERSET_OAUTH_KEY='Your Google OAuth key'
SUPERSET_OAUTH_SECRET='Your Google OAuth secret'
SUPERSET_OAUTH_WHITELIST='@sample.com'
```


## Examples

Navigate to the [`examples`](./examples) directory to view examples of how to configure Superset with MySQL, PostgreSQL, or SQLite.


## Configuration

Follow the [instructions](https://superset.incubator.apache.org/installation.html#configuration) provided by Apache Superset for writing your own `superset_config.py`. Place this file in a local directory and mount this directory to `/etc/superset` inside the container. This location is included in the image's `PYTHONPATH`. Mounting this file to a different location is possible, but it will need to be in the `PYTHONPATH`.

View the contents of the [`examples`](./examples) directory to see some simple `superset_config.py` samples.


## Database Initialization

After starting the Superset server, initialize the database with an admin user and Superset tables using the `superset-init` helper script:

```bash
docker run --detach --name superset [options] amancevice/superset
docker exec -it superset superset-init
```


## Upgrading

Upgrading to a newer version of superset can be accomplished by re-pulling `amancevice/superset`at a specified superset version or `latest` (see above for more on this). Remove the old container and re-deploy, making sure to use the correct environmental configuration. Finally, ensure the superset database is migrated up to the head:

```bash
# Pull desired version
docker pull amancevice/superset

# Remove the current container
docker rm -f superset-old

# Deploy a new container ...
docker run --detach --name superset-new [options] amancevice/superset

# Upgrade the DB
docker exec superset-new superset db upgrade
```
