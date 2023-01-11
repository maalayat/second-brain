docker run -it ubuntu

docker ps

docker logs CONTAINER_ID

docker logs -f CONTAINER_ID

docker stop CONTAINER_ID

docker ps -a


docker build -t "php-hello-world" .
docker run --rm -p 8000:80 -it "php-hello-world"
docker run --rm -p 8000:80 -it "php-hello-world"


docker build -t "cmd-vs-entrypoint" .
docker run --rm -it cmd-vs-entrypoint # if there is CMD
docker run --rm -it cmd-vs-entrypoint /etc/os-release
docker run --rm -it cmd-vs-entrypoint /etc/passwd


docker run --rm -e DISPLAY_ERRORS=Off -p 8000:80 -it "hello-world-php-ini


docker run --rm --restart=always -p 8000:80 -it "hello-world-php-ini
no
on-failure[:max-retries]
always
unless-stopped


docker run --rm --cpus=1.5 --memory=500m -p 8000:80 -lhhnd "hello-world-php-ini


docker run --name vol-test -v /data -it ubuntu
docker inspect -f "{{json .Mounts}}" vol-test
docker volume create codely-volume
docker volume ls
docker rm vol-test
docker run --name vol-test -v codely-volumne:/data -it ubuntu
docker run --name vol-test -v $PWD:/data -it ubuntu

docker run -p 8000:80 -v $PWD:/var/www/html -it php:7.2-apache

docker pull githubuser/image_name


docker compose up
docker compose up --build
docker compose logs -f app
docker compose top
docker compose ps
docker compose up -d # modo detached

docker compose stop
docker compose down

docker network ls


## Mysql docker
```
docker run --name my-mysql -p3306:3306 -e MYSQL_ROOT_PASSWORD=password -d mysql:8.0.31
```
