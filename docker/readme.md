# Docker README
This is a quick/dirty setup for building and running this in a container (with an optional containerized database). Note, this will get you up and running, but should not be directly used for a production deployment.

## Assumptions
- You have [Docker](https://docs.docker.com/get-docker/) (or similar) installed
- You have [Docker compose](https://docs.docker.com/compose/install/) installed

## Building image
From the root of the git-cloned project, run:

```bash
docker build -t meshtastic-map/core:dev -f docker/Dockerfile --no-cache .
```

The output (when complete) should look something like this:

```
[+] Building 10.7s (10/10) FINISHED            docker:default
 => [internal] load .dockerignore
 => => transferring context: 2B
 => [internal] load build definition from Dockerfile
 => => transferring dockerfile: 241B
 => [internal] load metadata for docker.io/library/node:21-alpine3.18
 => [1/5] FROM docker.io/library/node:21-alpine3.18
 => [internal] load build context
 => => transferring context: 22.51kB
 => CACHED [2/5] WORKDIR /app
 => [3/5] COPY . .
 => [4/5] COPY docker/entrypoint.sh entrypoint.sh
 => [5/5] RUN npm install
 => exporting to image
 => => exporting layers
 => => writing image sha256:7497ce86c4359e38843b04961c59d2a2c65e4005089c754ad2b815675452eb4a 
 => => naming to docker.io/meshtastic-map/core:dev 
```

## Generating random DB password
To help secure environment, create a randomly generated password for your database.  **Note: only run this once, overwriting this file on an existing install will render it unusable.**  Store this password in `docker/.env`.

```bash

echo MARIADB_ROOT_PASSWORD=$(openssl rand -base64 32 | tr -dc 'a-zA-Z0-9') > docker/.env
```

## Overriding MQTT connection information
Take a look at the `services.meshtastic-map.environment` section found in the `docker-compose.yml`, and you can override them either in that file, or by [extending with an overrides file](https://docs.docker.com/compose/multiple-compose-files/merge/).

## Running with Docker compose
Once you've built the image and generate your password, launch the stack via `docker compose` from within the `docker` directory:

```
cd docker
docker compose up -d
```

Then point your browser at `localhost:8080`!