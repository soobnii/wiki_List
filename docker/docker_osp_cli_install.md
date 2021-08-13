## OSP CLI tool 설치 및 테스트

## 1. 도커머신에 도커서버(vm) 생성 및 구동 
``` 
$ docker-machine create --driver virtualbox manager
$ docker-machine start manager
```

## 2. 통신을 위한 환경변수 셋팅
```
$ docker-machine env manager

=> 출력값 복사 후 ENTER
EX)
SET DOCKER_TLS_VERIFY=1
SET DOCKER_HOST=tcp://192.168.99.102:2376
SET DOCKER_CERT_PATH=C:\Users\user\.docker\machine\machines\manager
SET DOCKER_MACHINE_NAME=manager
SET COMPOSE_CONVERT_WINDOWS_PATHS=true
REM Run this command to configure your shell:
REM     @FOR /f "tokens=*" %i IN ('"C:\ProgramData\chocolatey\lib\docker-machine\bin\docker-machine.exe" env manager') DO @%i
```

## 3. 도커 이미지 pull -> 컨테이너 구동
** 볼륨 경로 => c는 반드시 소문자

```
$ docker pull jmcvea/openstack-client
$ docker run -ti --rm --volume="/c/Users/user/Desktop/data:/data" jmcvea/openstack-client
```

## 3. keystonerc 파일 생성
** 오픈스택 구동에 필요한 환경변수 셋팅

```
 / # vi /data/keystonerc.sh
```

** `접속할 오픈스택 IP : 192.168.63.150`  <br>
** `USERNAME/PW : admin/cone@234`
```
unset OS_SERVICE_TOKEN
export OS_USERNAME='admin'
export OS_PASSWORD='cone@234'
export OS_REGION_NAME=RegionOne
export OS_AUTH_URL=http://192.168.63.150:5000/v3
export PS1='[\u@\h \W(keystone_admin)]\$ '

export OS_PROJECT_NAME=admin
export OS_USER_DOMAIN_NAME=Default
export OS_PROJECT_DOMAIN_NAME=Default
export OS_IDENTITY_API_VERSION=3       
```

## 4. 확인 및 테스트 
```
/ # . /data/keystonerc.sh
[root@bfe0694e5cd3 data(keystone_admin)]# openstack network list
```

