# docker nginx vhost
![image](https://github.com/dana096/docker-nginx-vhost/assets/145534055/97669887-713c-4908-86d6-91498d47764d)

<br>

# load-balancing
https://www.nginx.com/resources/glossary/load-balancing/

<br>

# step 1
- docker rm * rmi
```
$ sudo docker images
REPOSITORY   TAG       IMAGE ID   CREATED   SIZE
$ sudo docker ps -a
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

<br>

# step 2
```
$ docker run -itd -p 8002:80 --name serv-a nginx
$ docker run -itd -p 8003:80 --name serv-b nginx
$ docker run -itd -p 8001:80 --name lb nginx:latest

sudo docker ps -a
CONTAINER ID   IMAGE          COMMAND                  CREATED              STATUS              PORTS                                   NAMES
bc8d24d12c4f   nginx          "/docker-entrypoint.…"   2 seconds ago        Up 1 second         0.0.0.0:8003->80/tcp, :::8003->80/tcp   serv-b
ea1b184ec4d7   nginx:latest   "/docker-entrypoint.…"   About a minute ago   Up About a minute   0.0.0.0:8001->80/tcp, :::8001->80/tcp   lb
3ac517b5245e   nginx          "/docker-entrypoint.…"   About a minute ago   Up About a minute   0.0.0.0:8002->80/tcp, :::8002->80/tcp   serv-a
```

<br>

# ref
- https://hub.docker.com/
