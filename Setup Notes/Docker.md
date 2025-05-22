
### Docker Build

It builds a **Docker image** from a `Dockerfile` in the current directory (`.`) and tags the resulting image as `my-app`.


```
docker build -t my-app .
```

Sample `Dockerfile`:

```
FROM node:18
WORKDIR /app
COPY . .
RUN npm install
CMD ["npm", "start"]
```

Docker will:

1. Use the `node:18` base image.
2. Copy your current folder into the container.
3. Install dependencies.
4. Set the default command to `npm start`.
5. Create an image named `my-app`.


### Docker Run 

**Runs a Docker container** from a specific image in **detached mode**, and **maps ports** so you can access the app from your host system.

exhibit A: 
```
docker run -d -p 9000:80 <image>:<tag>
```

- `docker run`: Starts a new container.
- `-d`: Detached mode — runs the container in the background.
- `-p 9000:80`: Maps **port 80 inside the container** to **port 9000 on your host**. So, visiting `http://localhost:9000` hits port 80 in the container.
- `<image>:<tag>`: Specifies the Docker image and its tag to run (e.g., `nginx:latest`, `my-app:v1`).

exhibit B:
You can pass multiple variables by repeating `-e`:

```
docker run -d -p 9000:80 \
  -e NODE_ENV=production \
  -e API_KEY=abcdef123 \
  my-app:latest
```


You can also load variables from a file:

```
docker run -d -p 9000:80 --env-file .env my-app:latest
```


### Docker ps

It **lists all containers** — including Running container, Stopped containers, Exited containers, Containers that failed to start


```
docker ps -a
```


To See Only Running Containers:

```
docker ps
```


### Docker prune

**cleans up** unused Docker **objects** to free up disk space

```
docker system prune
```

Remove only **stopped containers**

```
docker container prune
```

Remove only **unused images**:

```
docker image prune
```

Remove only **unused volumes**:

```
docker volume prune
```

Remove only **unused networks**:
```
docker network prune
```


### Docker compose

This runs Docker Compose with a custom project name and a specific YAML file.

```
docker-compose --project-name projects -f file.yaml up
```

- `docker-compose`: CLI tool to run multi-container Docker applications.
- `--project-name projects`: Sets the project name to `projects`. This affects the container, network, and volume names (they'll be prefixed with `projects_`).
- `-f file.yaml`: Uses the specified Compose file (`file.yaml`) instead of the default `docker-compose.yml`.
- `up`: Builds, (re)creates, and starts all the services defined in the file.

 If your `file.yaml` contains:
 
```
version: '3.8'
services:
  web:
    image: nginx
    ports:
      - "8080:80"
```

then running:

```
docker-compose --project-name projects -f file.yaml up
```

Will:
- Start an Nginx container named `projects_web_1`
- Map port 8080 on your machine to port 80 in the container

`-d`: Run in **detached mode** (in background)

```
docker-compose --project-name projects -f file.yaml up -d
```

`--build`: Rebuild images before starting

```
docker-compose --project-name projects -f file.yaml up --build
```
