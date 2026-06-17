 - [10 best practices to containerize Node.js web applications with Docker](https://snyk.io/blog/10-best-practices-to-containerize-nodejs-web-applications-with-docker/)


# Introduction to Containers

## Creating a container from an image

To create and run a container from an image, use:

`docker container run hello-world`

If the image isn't available locally, Docker pulls it from the registry first:

1. Unable to find image 'hello-world:latest' locally
2. latest: Pulling from library/hello-world
3. b8dfde127a29: Pull complete
4. Digest: sha256:5122f6204b6a3596e048758cabba3c46b1c937a46b5be6225b835d091b90e46c
5. Status: Downloaded newer image for hello-world:latest

## Running an interactive container

Let's start an Ubuntu container and open a shell inside it:

`docker container run -it ubuntu bash`

```bash
Unable to find image 'ubuntu:latest' locally
latest: Pulling from library/ubuntu
6f5c5aa4e145: Pull complete
1c24335ddd46: Pull complete
Digest: sha256:f3d28607ddd78734bb7f71f117f3c6706c666b8b76cbff7c9ff6e5718d46ff64
Status: Downloaded newer image for ubuntu:latest
root@b050ce5be5b1:/# mkdir /usr/src/app
root@b050ce5be5b1:/# touch /usr/src/app/index.js
root@7c9837452300:~# cd /usr/src/app && ls
index.js
root@b050ce5be5b1:/usr/src/app# exit
exit
```

The `-it` flag keeps an interactive terminal open so we can work inside the container.

## Running a one-off command

`docker container run --rm ubuntu ls -la`

```bash
total 60
drwxr-xr-x   1 root root 4096 Jun 15 09:09 .
drwxr-xr-x   1 root root 4096 Jun 15 09:09 ..
-rwxr-xr-x   1 root root    0 Jun 15 09:09 .dockerenv
drwxr-xr-x   2 root root 4096 Apr 21 17:23 .rock
lrwxrwxrwx   1 root root    7 Apr 20 08:46 bin -> usr/bin
drwxr-xr-x   2 root root 4096 Apr 20 08:46 boot
drwxr-xr-x   5 root root  340 Jun 15 09:09 dev
drwxr-xr-x   1 root root 4096 Jun 15 09:09 etc
drwxr-xr-x   3 root root 4096 Apr 21 17:23 home
lrwxrwxrwx   1 root root    7 Apr 20 08:46 lib -> usr/lib
lrwxrwxrwx   1 root root    9 Apr 20 08:46 lib64 -> usr/lib64
drwxr-xr-x   2 root root 4096 Apr 21 02:05 media
drwxr-xr-x   2 root root 4096 Apr 21 02:05 mnt
drwxr-xr-x   2 root root 4096 Apr 21 02:05 opt
dr-xr-xr-x 233 root root    0 Jun 15 09:09 proc
drwx------   2 root root 4096 Apr 21 17:23 root
drwxr-xr-x   4 root root 4096 Apr 21 17:23 run
lrwxrwxrwx   1 root root    8 Apr 20 08:46 sbin -> usr/sbin
drwxr-xr-x   2 root root 4096 Apr 21 02:05 srv
dr-xr-xr-x  11 root root    0 Jun 15 09:02 sys
drwxrwxrwt   2 root root 4096 Apr 21 02:06 tmp
drwxr-xr-x  12 root root 4096 Apr 21 17:23 usr
drwxr-xr-x  11 root root 4096 Apr 21 17:23 var
```

The `--rm` flag automatically removes the container once the command finishes.

## Listing and managing containers

List all containers, including stopped ones:

`docker container ls -a` (or the shorter `docker ps -a`)

```bash
CONTAINER ID   IMAGE     COMMAND   CREATED          STATUS          PORTS     NAMES
fb9b2b62fcfa   ubuntu    "bash"    26 seconds ago   Up 25 seconds             modest_williams
```

Start an existing container:

`docker start modest_williams`

Note that you can't interact with it unless you attach an input stream with `-i`.

Stop a running container:

`docker kill modest_williams`

Start it again, this time interactively:

`docker start -i modest_williams`

```bash
root@fb9b2b62fcfa:/# apt-get update
root@fb9b2b62fcfa:/# apt-get -y install nano
root@fb9b2b62fcfa:/# nano /usr/src/app/index.js
```

## Installing Node.js inside the container

```bash
root@fb9b2b62fcfa:/# apt-get -y install curl
root@fb9b2b62fcfa:/# curl -sL https://deb.nodesource.com/setup_24.x | bash
root@fb9b2b62fcfa:/# apt install -y nodejs

root@fb9b2b62fcfa:/# node /usr/src/app/index.js
Hello World
```

## Saving the container as a new image

Once the container is set up the way you want, commit it to a new image:

`docker commit modest_williams hello-node-world`

You can then spin up fresh containers from that image:

`docker run -it hello-node-world bash`


# Building and configuring environments

## Dockerfile

```dockerfile
WORKDIR /usr/src/app

COPY --chown=node:node . .

RUN npm ci --omit=dev

USER node

ENV PORT=3001

CMD ["npm", "start"]
```

`COPY --chown=node:node . .` copies the files and makes the `node` user their owner.

`USER node` runs as the non-root `node` user for better security.

`.dockerignore`

```
.dockerignore
.gitignore
node_modules
Dockerfile
```

Build the image with:

`docker build -t express-server .`

`-t` names (tags) the image, and `.` tells Docker to use the current directory as the build context.

Run a container from the image with:

`docker run -p 3001:3001 express-server`

`-p` publishes a port: it maps a port on the host machine to a port inside the container, in the form `host:container`.

## Using Docker compose:

`docker-compose.yml`

```yaml
services:
  app:                    # The name of the service
    image: express-server # Declares which image to use
    build: .              # Declares where to build if image is not found
    ports:                # Declares the ports to publish
      - 3001:3001
```

`docker compose up`: To build and run the application.

`docker compose up --build`: To rebuild the images.

`docker compose up -d`: To run the application in the background.

`docker compose down`: To close.


## Bind mount and initializing the database

```yaml
services:
  mongo:
    image: mongo
    ports:
      - 3456:27017
    environment:
      MONGO_INITDB_ROOT_USERNAME: root
      MONGO_INITDB_ROOT_PASSWORD: example
      MONGO_INITDB_DATABASE: the_database
    volumes:
      - ./mongo/mongo-init.js:/docker-entrypoint-initdb.d/mongo-init.js
      - ./mongo_data:/data/db
```

Start the services in the background (`-f` selects the compose file, `-d` runs in detached mode):

`docker compose -f docker-compose.dev.yml up -d`

Stop and remove the containers and network:

`docker compose -f docker-compose.dev.yml down`

Do the same, but also delete the named volumes (this wipes the stored data):

`docker compose -f docker-compose.dev.yml down --volumes`

See where the Mongo image stores its data: [Where to Store Data](https://hub.docker.com/_/mongo/#where-to-store-data).

## Persisting data

bind mount: -v FILE-IN-HOST:FILE-IN-CONTAINER

The key to software development in containers.

Changes to either file will be available in the other.

- [bind mount](https://docs.docker.com/engine/storage/bind-mounts/)
  ```yaml
  services:
    mongo:
      volumes:
        - ./mongo_data:/data/db
  ```

- [volume](https://docs.docker.com/engine/storage/volumes/)
  ```yaml
  services:
    mongo:
      volumes:
        - mongo_data:/data/db
    volumes:
      mongo_data:
  ```


## [docker container exec](https://docs.docker.com/reference/cli/docker/container/exec/)

[`exec`](https://docs.docker.com/reference/cli/docker/container/exec/): The docker exec command runs a new command in a running container.

```bash
docker exec -it 81 bash

root@812290766cda:/# mongosh -u root -p example
```

Example 

```yaml
services:
  mongo:
    image: mongo
    ports:
      - 3456:27017
    environment:
      MONGO_INITDB_ROOT_USERNAME: root
      MONGO_INITDB_ROOT_PASSWORD: example
      MONGO_INITDB_DATABASE: the_database
    volumes:
      - ./mongo/mongo-init.js:/docker-entrypoint-initdb.d/mongo-init.js
      - ./mongo_data:/data/db

  redis:
    image: redis
    ports:
      - 6379:6379
    command: ['redis-server', '--appendonly', 'yes']
    volumes:
      - ./redis_data:/data
```

# Basics of Container Orchestration

## React in container

```dockerfile
  FROM node:24 AS build-stage

  WORKDIR /usr/src/app

  COPY . .

  ENV VITE_BACKEND_URL=http://localhost:3000

  RUN npm ci

  RUN npm run build

  FROM nginx:1.29-alpine

  COPY --from=build-stage /usr/src/app/dist /usr/share/nginx/html
```

`docker build -t todo-frontend-image .`

`docker run --name todo-frontend -d -p 8080:80 todo-frontend-image`

Multi-stage builds:

```dockerfile
  FROM node:24 AS test-stage
  WORKDIR /usr/src/app
  COPY . .
  ENV VITE_BACKEND_URL=http://localhost:3000
  RUN npm ci
  RUN npm run test

  FROM node:24 AS build-stage
  WORKDIR /usr/src/app
  COPY --from=test-stage /usr/src/app /usr/src/app
  RUN npm run build

  FROM nginx:1.29-alpine
  COPY --from=build-stage /usr/src/app/dist /usr/share/nginx/html
```

## Development in containers

dev.Dockerfile

```dockerfile
FROM node:24
WORKDIR /usr/src/app
COPY . .
# Change npm ci to npm install since we are going to be in development mode
RUN npm install
CMD ["npm", "run", "dev", "--", "--host"]
```
docker-compose.dev.yml

```yaml
services:
  app:
    image: todo-frontend-dev
    build:
      context: . # The context will pick this directory as the "build context"
      dockerfile: dev.Dockerfile
    volumes:
      - ./:/usr/src/app # The path can be relative, so ./ is enough to say "the same location as the docker-compose.yml"
      - /usr/src/app/node_modules # An anonymous volume that keeps the container's node_modules, so the bind mount above doesn't hide it with the host's.

    ports:
      - 5173:5173
    container_name: todo-frontend-dev # This will name the container hello-front-dev
```

`docker compose -f docker-compose.dev.yml up`

Install the new dependency inside the container:

`docker exec todo-frontend-dev npm install axios`

## Communication between containers in a Docker network

```yaml
services:
  express-server:
    environment:
      MONGO_URL: mongodb://root:example@mongo:27017/the_database?authSource=admin
      REDIS_URL: redis://redis:6379

  mongo:
    image: mongo
    ports:
      - 3456:27017

  redis:
    image: redis
    ports:
      - 6379:6379

```

 -[Networking in Compose  Compose](https://docs.docker.com/compose/how-tos/networking/)

## Connect the services, todo-frontend with todo-backend

nginx.dev.conf

```nginx
# events is required, but defaults are ok
events { }

# A http server, listening at port 80
http {
  server {
    listen 80;

    # Requests starting with root (/) are handled
    location / {
      # The following 3 lines are required for the hot reload to work
      proxy_http_version 1.1;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection 'upgrade';

      # Requests are directed to http://app:5173
      proxy_pass http://app:5173;
    }

    location /api/ {
      proxy_http_version 1.1;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection 'upgrade';

      # Even though the browser will send a GET request to /api/todos/1 we want the Nginx to proxy the request to /todos/1. Do this by adding a trailing slash / to the URL at the end of proxy_pass.
      proxy_pass http://server:3000/;
    }
  }
}
```

docker-compose.dev.yml

```yaml
services:
  mongo:
    image: mongo
    environment:
      MONGO_INITDB_ROOT_USERNAME: root
      MONGO_INITDB_ROOT_PASSWORD: example
      MONGO_INITDB_DATABASE: the_database
    volumes:
      - ./todo-backend/mongo/mongo-init.js:/docker-entrypoint-initdb.d/mongo-init.js
      - ./todo-backend/mongo_data:/data/db
    container_name: mongo

  redis:
    image: redis
    command: ['redis-server', '--appendonly', 'yes']
    volumes:
      - ./todo-backend/redis_data:/data
    container_name: redis

  server:
    image: todo-backend-dev
    build:
      context: ./todo-backend
      dockerfile: dev.Dockerfile
    volumes:
      - ./todo-backend:/usr/src/app
      - /usr/src/app/node_modules

    environment:
      PORT: 3000
      MONGO_URL: mongodb://root:example@mongo:27017/the_database?authSource=admin
      REDIS_URL: redis://redis:6379
    container_name: todo-backend-dev
    depends_on:
      - mongo
      - redis

  app:
    image: todo-frontend-dev
    build:
      context: ./todo-frontend
      dockerfile: dev.Dockerfile
    environment:
      VITE_BACKEND_URL: /api
    volumes:
      - ./todo-frontend:/usr/src/app
      - /usr/src/app/node_modules
    container_name: todo-frontend-dev

  nginx:
    image: nginx:1.29
    volumes:
      - ./nginx.dev.conf:/etc/nginx/nginx.conf:ro
    ports:
      - 8080:80
    container_name: reverse-proxy
    depends_on:
      - app
      - server
```