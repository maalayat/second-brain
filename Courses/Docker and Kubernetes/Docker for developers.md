### Basic commands
```bash
docker ps
docker logs CONTAINER_ID
docker logs -f CONTAINER_ID
docker stop CONTAINER_ID
docker ps -a
```
### Build
```bash
docker build -t "php-hello-world" .
docker run --rm -p 8000:80 -it "php-hello-world"
docker run --rm -p 8000:80 -it "php-hello-world"
```
### Run
```bash
docker build -t "cmd-vs-entrypoint" .
docker run --rm -it cmd-vs-entrypoint # if there is CMD
docker run --rm -it cmd-vs-entrypoint /etc/os-release
docker run --rm -it cmd-vs-entrypoint /etc/passwd
```

```bash
docker run --rm -e DISPLAY_ERRORS=Off -p 8000:80 -it "hello-world-php-ini
```

```bash
docker run --rm --restart=always -p 8000:80 -it "hello-world-php-ini
no
on-failure[:max-retries]
always
unless-stopped
```

```bash
docker run --rm --cpus=1.5 --memory=500m -p 8000:80 -lhhnd "hello-world-php-ini
```

### Volume
```bash
docker run --name vol-test -v /data -it ubuntu
docker inspect -f "{{json .Mounts}}" vol-test
docker volume create codely-volume
docker volume ls
docker rm vol-test
docker run --name vol-test -v codely-volumne:/data -it ubuntu
docker run --name vol-test -v $PWD:/data -it ubuntu
```

```bash
docker run -p 8000:80 -v $PWD:/var/www/html -it php:7.2-apache
```

```bash
docker pull githubuser/image_name
```
### Compose
```bash
docker compose up
docker compose up --build
docker compose logs -f app
docker compose top
docker compose ps
docker compose up -d # modo detached
```

```bash
docker compose -f docker-compose.yml up
```

```bash
docker compose stop
docker compose down
```
### Network
```bash
docker network ls
```
### Context
```bash
docker context ls
NAME              TYPE            DOCKER ENDPOINT 
default           moby             unix:///var/run/docker.sock
desktop-linux *   moby       unix:///home/user/.docker/desktop/docker.sock
```
### Use a different context
```bash
docker context use default
```

### Mysql docker
```bash
docker run --name my-mysql -p3306:3306 -e MYSQL_ROOT_PASSWORD=password -d mysql:8.0.31
```
### Docker desktop unix socker

```bash
sudo ln -s /home/user/.docker/desktop/docker.sock /var/run/
```

---

## Links 
- [docker-compose standalone](https://docs.docker.com/compose/install/other/)
- https://stackoverflow.com/questions/66514436/difference-between-docker-compose-and-docker-compose