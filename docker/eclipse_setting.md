## 1)virtualbox로 도커머신에 도커서버 생성 (이름: manager) -> 구동 후 접속
```
docker-machine create --driver virtualbox manager
docker-machine start manager
docker ssh manager
```

## 2)도커허브에 등록된 centos7-vnc-git 이미지 pull 해서 repository 에 업로드
```
docker pull moricom/centos7-vnc-git
```

## 3)docker registory 에 등록한 이미지 컨테이너로 띄우기
```
docker run -it --user 0 -d -p 5901:5901 -p 6901:6901 --name centos7-vnc-git moricom/centos7-vnc-git
```

## 4)폴더 생성 -> dockerfile 생성(eclipse/tomcat 8.5/jdk8 설치)
```
mkdir test_eclipse
vi dockerfile
```

```
FROM moricom/centos7-vnc-git

MAINTAINER soobnii

RUN mkdir app

WORKDIR app

USER 0
RUN wget http://apache.tt.co.kr/tomcat/tomcat-8/v8.5.56/bin/apache-tomcat-8.5.56.tar.gz && \
tar -xzvf ./apache-tomcat-8.5.56.tar.gz

RUN wget http://ftp.jaist.ac.jp/pub/eclipse/technology/epp/downloads/release/2020-06/R/eclipse-jee-2020-06-R-linux-gtk-x86_64.tar.gz && \
tar -zxvf eclipse-jee-2020-06-R-linux-gtk-x86_64.tar.gz

RUN wget https://github.com/frekele/oracle-java/releases/download/8u212-b10/jdk-8u212-linux-x64.tar.gz && \
tar -zxvf jdk-8u212-linux-x64.tar.gz

# jdk 경로 설정
RUN perl -p -i -e '$.==3 and print "-vm/headless/app/jdk1.8.0_212/bin/java "' /headless/app/eclipse/eclipse.ini

# 해당 경로 모든 하위 폴더/파일에 권한 부여
RUN chmod -R 755 /headless/app

USER 1000100001
```

## 5)dockerfile 로 이미지 빌드하고 컨테이너 띄우기
```
docker build -t soobnii/centos7_eclipse_tomcat8_test .
docker run -it --user 0 -d -p 5901:5901 -p 6901:6901 --name centos7-ecilpse-tomcat8 soobnii/centos7_eclipse_tomcat8_test
```

## 6)컨테이너 실행 후 vnc 접속해서 제대로 됐는지 확인

## 6-2)포트포워딩 후 외부PC에서 접속 확인
```
netsh interface portproxy show v4tov4
netsh interface portproxy add v4tov4 listenport=5901 listenaddress=192.168.2.82 connectport=5901 connectaddress=192.168.99.102
netsh interface portproxy delete v4tov4 listenport=5901 listenaddress=192.168.2.82
```

## 7)로그인 후 도커허브에 push
```
docker login
docker push soobnii/centos7_eclipse_tomcat8_test
```

## ##vnc 접속 후 cli로 이클립스 개발환경 셋팅
```
mkdir app

cd app
wget http://apache.tt.co.kr/tomcat/tomcat-8/v8.5.56/bin/apache-tomcat-8.5.56.tar.gz && \
        tar xzvf ./apache-tomcat-8.5.56.tar.gz

wget http://ftp.jaist.ac.jp/pub/eclipse/technology/epp/downloads/release/2020-06/R/eclipse-jee-2020-06-R-linux-gtk-x86_64.tar.gz && \
       tar -zxvf eclipse-jee-2020-06-R-linux-gtk-x86_64.tar.gz 
	   
wget https://github.com/frekele/oracle-java/releases/download/8u212-b10/jdk-8u212-linux-x64.tar.gz && \
		tar -zxvf jdk-8u212-linux-x64.tar.gz 

perl -p -i -e '$.==3 and print "-vm\n/headless/app/jdk1.8.0_212/bin/java \n"' /headless/app/eclipse/eclipse.ini
```
