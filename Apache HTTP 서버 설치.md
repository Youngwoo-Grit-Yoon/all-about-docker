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

## 3. 실행된 컨테이너로 진입
```text
$ docker exec -it apache-http-2.4.58 /bin/bash
(pyenv) [root@bb053394900d ~]#
```

## 4. 기존 HTTPD 설정 파일 백업
```text
(pyenv) [root@bb053394900d ~]# cp -arp /etc/httpd /etc/httpd_2.4.58
```

## 5. yum epel-release 설치 및 업그레이드
```text
(pyenv) [root@bb053394900d ~]# yum install -y epel-release
(pyenv) [root@bb053394900d ~]# yum update -y 
```

## 6. wget 설치
```text
(pyenv) [root@bb053394900d ~]# yum install -y wget
```

## 7. 새로운 레포지토리 설치
```text
(pyenv) [root@bb053394900d ~]# cd /etc/yum.repos.d && wget https://repo.codeit.guru/codeit.el`rpm -q --qf "%{VERSION}" $(rpm -q --whatprovides redhat-release)`.repo
```

## 8. yum으로 다운로드 가능한 httpd 확인
```text
(pyenv) [root@bb053394900d ~]# yum list httpd
```
<img width="1786" alt="image" src="https://github.com/Youngwoo-Grit-Yoon/all-about-docker/assets/101490683/fd2c4edb-eb14-4b86-afbf-520676726330">

## 9. 설치 진행
```text
(pyenv) [root@bb053394900d ~]# yum install -y httpd*
```

## 10. 설치된 버전 확인
```text
(pyenv) [root@bb053394900d ~]# httpd -V
Server version: Apache/2.4.58 (codeit)
Server built:   Oct 19 2023 10:27:37
Server's Module Magic Number: 20120211:129
Server loaded:  APR 1.7.4, APR-UTIL 1.6.3, PCRE 10.23 2017-02-14
Compiled using: APR 1.7.4, APR-UTIL 1.6.3, PCRE 10.23 2017-02-14
Architecture:   64-bit
Server MPM:     event
  threaded:     yes (fixed thread count)
    forked:     yes (variable process count)
Server compiled with....
 -D APR_HAS_SENDFILE
 -D APR_HAS_MMAP
 -D APR_HAVE_IPV6 (IPv4-mapped addresses enabled)
 -D APR_USE_PROC_PTHREAD_SERIALIZE
 -D APR_USE_PTHREAD_SERIALIZE
 -D SINGLE_LISTEN_UNSERIALIZED_ACCEPT
 -D APR_HAS_OTHER_CHILD
 -D AP_HAVE_RELIABLE_PIPED_LOGS
 -D DYNAMIC_MODULE_LIMIT=256
 -D HTTPD_ROOT="/etc/httpd"
 -D SUEXEC_BIN="/usr/sbin/suexec"
 -D DEFAULT_PIDLOG="run/httpd.pid"
 -D DEFAULT_SCOREBOARD="logs/apache_runtime_status"
 -D DEFAULT_ERRORLOG="logs/error_log"
 -D AP_TYPES_CONFIG_FILE="conf/mime.types"
 -D SERVER_CONFIG_FILE="conf/httpd.conf"
```

## 11. MPM 변경
```text
(pyenv) [root@bb053394900d ~]# vi /etc/httpd/conf.modules.d/00-mpm.conf
주석 처리
LoadModule mpm_event_module modules/mod_mpm_event.so -> #LoadModule mpm_event_module modules/mod_mpm_event.so
주석 해제
#LoadModule mpm_prefork_module modules/mod_mpm_prefork.so -> LoadModule mpm_prefork_module modules/mod_mpm_prefork.so
```

## 12. 컨테이너를 통째로 Docker 이미지로 생성
```text
(pyenv) [root@bb053394900d ~]# docker commit apache-http-2.4.58 push-base-image:1.1
samsung-securities-push-base-image   1.1         847274438d43   30 seconds ago   1.52GB
samsung-securities-push-base-image   1.0         bcec2791389f   23 months ago    1.01GB
```
