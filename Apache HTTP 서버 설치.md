# Apache HTTP 서버 설치
해당 문서는 기존 2.4.6 버전으로 설치되어 있는 아파치 서버를 2.4.58로 재설치하는 방법을 설명한다. 재설치하는 이유는 2.4.6 버전에서 취약점이 발견되었기 때문이다.
## 1. Apache HTTP 2.4.6이 설치되어 있는 CentOS Docker 이미지 확인
```text
$ docker images
REPOSITORY                           TAG         IMAGE ID       CREATED         SIZE
push-base-image   1.0         bcec2791389f   23 months ago   1.01GB
```
## 2. 해당 이미지 실행
```text
$ docker run --name apache-http-2.4.58 -d --privileged push-base-image:1.0 /usr/sbin/httpd -D FOREGROUND
bb053394900d2aaf1998c203ea8160087f6a94d6364552182a0cf548700b8bc4
$ docker ps
CONTAINER ID   IMAGE                 COMMAND                  CREATED         STATUS         PORTS     NAMES
bb053394900d   push-base-image:1.0   "/usr/sbin/httpd -D …"   5 seconds ago   Up 2 seconds             apache-http-2.4.58
```
