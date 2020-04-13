## Docker

### Containers

```
docker container run hello-world
docker container run debian bash --version
docker ps
docker ps -a
docker container run --rm debian
docker container run -it debian bash
docker container run --name my-deb -it debian bash
docker container ls -a
docker container start -ai my-deb
docker container run -p 8080:80 nginx
docker container run -p 8080:80 -v /var/www/docker-app/curso-docker/ex-volume/not-found:/usr/share/nginx/html nginx
docker container run -p 8080:80 -v /var/www/docker-app/curso-docker/ex-volume/html:/usr/share/nginx/html nginx
```
###### - d or --detach to run detached
```
docker container run -d --name ex-daemon-basic -p 8080:80 -v /var/www/docker-app/curso-docker/ex-volume/html:/usr/share/nginx/html nginx
docker container stop ex-daemon-basic
docker container start ex-daemon-basic
docker container restart ex-daemon-basic
docker container ps
docker container ls
docker container list
docker container logs ex-daemon-basic
docker container inspect ex-daemon-basic
docker container exec ex-daemon-basic uname -or
```

### Images

```
docker image pull redis
docker image inspect redis:latest
docker image list
docker image tag redis:latest curso-redis
docker image rm  curso-redis
docker image build -t ex-simple-build .
docker container run -p 8080:80 ex-simple-build
docker container run ex-build-arg bash -c 'echo $S3_BUCKET'
docker image build --build-arg S3_BUCKET=myapp -t ex-build-arg .
docker inspect --format='{{.Config.Labels}}' ex-build-arg
```
```
docker container run -it -v $(pwd):/app -p 8080:8000 --name python-server ex-build-dev
docker container run -it --volumes-from=python-server debian cat /log/http-server.log
```

```
docker image tag ex-simple-build lourivaldo/simple-build:1.0
docker login --username=lourivaldo
docker image push lourivaldo/simple-build:1.0
```

### Network

```
docker network list
```
```
docker container run --rm alpine ash -c 'ifconfig'
docker container run --rm --net none alpine ash -c 'ifconfig'
```

```
docker container run -d --name container1 alpine sleep 1000
docker container exec -it container1 ifconfig
docker container run -d --name container2 alpine sleep 1000
docker container exec -it container2 ifconfig
docker container exec -it container1 ping 172.17.0.6
docker network create --driver bridge rede_nova
docker network list

docker container run -d --name container3 --net rede_nova alpine sleep 1000
docker container exec -it container3 ifconfig

docker container exec -it container3 ping 172.17.0.6
docker network connect bridge container3
docker container exec -it container3 ping 172.17.0.6
docker network disconnect bridge container3
docker container run -d --name container4 --net host alpine sleep 1000
docker container exec -it container4 ifconfig
```
 
```
docker-compose up
docker-compose exec db psql -U postgres -c '\l'
docker-compose up -d
docker-compose ps
docker-compose down
docker-compose logs -f -t
docker-compose exec db psql -U postgres -f /scripts/check.sql

```
