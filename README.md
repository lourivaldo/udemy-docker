##Docker

###Containers

```
sudo docker container run hello-world
sudo docker container run debian bash --version
sudo docker ps
sudo docker ps -a
sudo docker container run --rm debian
sudo docker container run -it debian bash
sudo docker container run --name my-deb -it debian bash
sudo docker container ls -a
sudo docker container start -ai my-deb
sudo docker container run -p 8080:80 nginx
sudo docker container run -p 8080:80 -v /var/www/docker-app/curso-docker/ex-volume/not-found:/usr/share/nginx/html nginx
sudo docker container run -p 8080:80 -v /var/www/docker-app/curso-docker/ex-volume/html:/usr/share/nginx/html nginx
```
###### - d or --detach to run detached
```
sudo docker container run -d --name ex-daemon-basic -p 8080:80 -v /var/www/docker-app/curso-docker/ex-volume/html:/usr/share/nginx/html nginx
sudo docker container stop ex-daemon-basic
sudo docker container start ex-daemon-basic
sudo docker container restart ex-daemon-basic
sudo docker container ps
sudo docker container ls
sudo docker container list
sudo docker container logs ex-daemon-basic
sudo docker container inspect ex-daemon-basic
sudo docker container exec ex-daemon-basic uname -or
```

###Images

```
sudo docker image pull redis
sudo docker image inspect redis:latest
sudo docker image list
sudo docker image tag redis:latest curso-redis
sudo docker image rm  curso-redis
sudo docker image build -t ex-simple-build .
sudo docker container run -p 8080:80 ex-simple-build
sudo docker container run ex-build-arg bash -c 'echo $S3_BUCKET'
sudo docker image build --build-arg S3_BUCKET=myapp -t ex-build-arg .
sudo docker inspect --format='{{.Config.Labels}}' ex-build-arg
```
```
sudo docker container run -it -v $(pwd):/app -p 8080:8000 --name python-server ex-build-dev
sudo docker container run -it --volumes-from=python-server debian cat /log/http-server.log
```

```
sudo docker image tag ex-simple-build lourivaldo/simple-build:1.0
sudo docker login --username=lourivaldo
sudo docker image push lourivaldo/simple-build:1.0
```

###Network

```
sudo docker network list
```
```
sudo docker container run --rm alpine ash -c 'ifconfig'
sudo docker container run --rm --net none alpine ash -c 'ifconfig'
```

```
sudo docker container run -d --name container1 alpine sleep 1000
sudo docker container exec -it container1 ifconfig
sudo docker container run -d --name container2 alpine sleep 1000
sudo docker container exec -it container2 ifconfig
sudo docker container exec -it container1 ping 172.17.0.6
sudo docker network create --driver bridge rede_nova
sudo docker network list

sudo docker container run -d --name container3 --net rede_nova alpine sleep 1000
sudo docker container exec -it container3 ifconfig

sudo docker container exec -it container3 ping 172.17.0.6
sudo docker network connect bridge container3
sudo docker container exec -it container3 ping 172.17.0.6
sudo docker network disconnect bridge container3
sudo docker container run -d --name container4 --net host alpine sleep 1000
sudo docker container exec -it container4 ifconfig
```
 
```
sudo docker-compose up
sudo docker-compose exec db psql -U postgres -c '\l'
sudo docker-compose up -d
sudo docker-compose ps
sudo docker-compose down
sudo docker-compose logs -f -t
sudo docker-compose exec db psql -U postgres -f /scripts/check.sql

```