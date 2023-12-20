# Apache Tomcat 컨테이너 배포
## 1. Tomcat 공식 이미지 가져오기
```text
$ docker pull tomcat:9-jdk17-temurin
```
## 2. Tomcat 컨테이너 실행
```text
$ docker run --name tomcat \
-it --rm \
-d \
-p 8888:8080 \
tomcat:9-jdk17-temurin
```
