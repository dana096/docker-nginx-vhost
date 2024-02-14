# docker nginx vhost
![image](https://github.com/dana096/docker-nginx-vhost/assets/145534055/97669887-713c-4908-86d6-91498d47764d)

<br>

# load-balancing
https://www.nginx.com/resources/glossary/load-balancing/

<br>

# Dockerfile
- Dockerfile 생성
```
$ cat Dockerfile

# lb
FROM nginx
COPY config/default.conf /etc/nginx/conf.d/

# serv-a / serv-b
FROM nginx
COPY index.html /usr/share/nginx/html
```

- lb/config/default.conf 코드 수정
```
upstream serv {
        server n1-1:80;
        server n1-2:80;
}
server {
        listen 80;

        location /
        {
                proxy_pass http://serv;
        }
}
```
- build & run
```
# n1-1 (serv-a)
$ sudo docker build -t ng-n-1:0.1.0 .
$ sudo docker run -d -p 9011:80 ng-n-1:0.1.0

# n1-2 (serv-b)
$ sudo docker build -t ng-n-2:0.1.0 .
$ sudo docker run -d -p 9012:80 ng-n-2:0.1.0

# n1-3 (lb)
$ sudo docker build -t ng-n-3:0.1.0 .
$ sudo docker run -d -p 9013:80 ng-n-3:0.1.0
```
- check
```
$ sudo docker ps -a
CONTAINER ID   IMAGE                 COMMAND                  CREATED          STATUS                   PORTS                                   NAMES
bc0c07bdc3a0   ng-n-3:0.1.0          "/docker-entrypoint.…"   3 minutes ago    Up 2 seconds             0.0.0.0:9013->80/tcp, :::9013->80/tcp   n1-3
63b9b4956ac8   ng-n-2:0.1.0          "/docker-entrypoint.…"   8 minutes ago    Up 8 minutes             0.0.0.0:9012->80/tcp, :::9012->80/tcp   n1-2
a7a1e2ad223d   ng-n-1:0.1.0          "/docker-entrypoint.…"   9 minutes ago    Up 9 minutes             0.0.0.0:9011->80/tcp, :::9011->80/tcp   n1-1
```

<br>

# network
- 네트워크 조회
```
$ docker network ls
```
- 네트워크 생성
```
$ sudo docker network create doc
```
- 네트워크에 컨테이너 연결 (connect)
```
$ sudo docker network connect doc n1-1
$ sudo docker network connect doc n1-2
$ sudo docker network connect doc n1-3
```
- 네트워크 상세 정보 (inspect)
```
$ sudo docker network inspect doc
```
- _**n1-3(lb) 가 연결되지 않는다면?**_
```
$ sudo docker start n1-3
```
- 컨테이너 n1-3(lb) 실행 <br>
[localhost:9013](http://localhost:9013/)http://localhost:9013/

<br>

# [ TEST ]

![image](https://github.com/dana096/docker-nginx-vhost/assets/145534055/56af85a6-5a97-4fdc-a1ab-6beca2e3528f)
![image](https://github.com/dana096/docker-nginx-vhost/assets/145534055/23bc9790-b2dd-4b48-b6e3-c2d90a6dd2a5)


