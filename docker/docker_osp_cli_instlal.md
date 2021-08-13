- 로컬 PC에 OSP CLI tool 설치

* 윈도우 환경에서 도커 설치

* cmd를 관리자로 열고 아래 명령어 수행

$ docker-machine start manager

$ docker-machine env manager

=> 위 명령어 수행 후 나오는 출력 내용 copy&paste 후 엔터


SET DOCKER_TLS_VERIFY=1
SET DOCKER_HOST=tcp://192.168.99.102:2376
SET DOCKER_CERT_PATH=C:\Users\user\.docker\machine\machines\manager
SET DOCKER_MACHINE_NAME=manager
SET COMPOSE_CONVERT_WINDOWS_PATHS=true
REM Run this command to configure your shell:
REM     @FOR /f "tokens=*" %i IN ('"C:\ProgramData\chocolatey\lib\docker-machine\bin\docker-machine.exe" env manager') DO @%i

$ docker-machine ssh manager 
$ docker pull jmcvea/openstack-client

$ docker run -ti --rm --volume="/c/Users/user/Desktop/data:/data" jmcvea/openstack-client

# cat /data/keystonerc.sh

unset OS_SERVICE_TOKEN

    export OS_USERNAME=ptlkim

    export OS_PASSWORD='cone@234'

    export OS_REGION_NAME=RegionOne

    export OS_AUTH_URL=http://192.168.60.90:5000/v3

    export PS1='[\u@\h \W(keystone_admin)]\$ '

 

export OS_PROJECT_NAME=admin

export OS_USER_DOMAIN_NAME=Default

export OS_PROJECT_DOMAIN_NAME=Default

export OS_IDENTITY_API_VERSION=3

/data # . keystonerc.sh

[root@bfe0694e5cd3 data(keystone_admin)]# openstack server list

+--------------------------------------+-------+--------+---------------------+-------+--------+

| ID                                   | Name  | Status | Networks            | Image | Flavor |

+--------------------------------------+-------+--------+---------------------+-------+--------+

| a6132b7b-2f2c-4410-9542-f76fb272be36 | node1 | ACTIVE | priv-svc=10.0.0.219 |       | test   |

+--------------------------------------+-------+--------+---------------------+-------+--------+