
### Docker 이미지 push 

#### 1. docker 가 설치된 VM 구동 및 접속
```
docker-machine start manager
docker-machine ssh manager
sudo su 
```

#### 2. 태그 변경
```
docker tag echo_test:latest soobnii/echo_test:v3.7
```

#### 3. 명령어로 이미지 검색 
```
sudo docker search tomcat
```

#### 4. hub.docker.com 에 업로드
```
docker push soobnii/echo_test
```

#### 이미지 전체 삭제
docker rmi $(docker images -q) -f   # -f : 강제 삭제

#### 이미지 실행 (-t: tag  / -d : detached mode 백그라운드모드)
sudo docker run -t -p 12345:12345 --name et --rm soobnii/echo_test:v3.7

#### 다른탭에서 접속
docker@manager:~$ nc 127.0.0.1 12345
 test12345
 test12345

#### 접속시 기존 탭에서 다음이 출력
server is started
Connected by ('172.17.0.1', 36820)  #172.17.0.1 도커 네트워크 ip 

#### 히스토리 확인  (해당 이미지를 만드는 동안 실행한 도커 명령어)
docker history soobnii/echo_test:v3.7

==========================================================================
#### 볼륨 마운트 해서 서비스 구축 (docker run -v <호스트경로>:<컨테이너 내 경로>:<권한>
#### 권한 :ro-읽기전용, rw-읽기 및 쓰기
docker run -d -p 80:80 --rm -v /var/www:/usr/share/nginx/html:ro nginx\

#### 포트 중복 에러 시, 도커 전체 멈추기 
docker stop $(docker ps -a -q)

#### 중지된 것 포함 모든 컨테이너 출력 (ls == ps), 현재 구동중인것만 : docker ps 
docker ps -a

#### 다른 탭에서~
sudo su
cd /var/www
#### 비어있는 경로에 index.html로 출력문 보내기(echo) 
echo test1234 > index.html
 
## 접속 확인
curl https://localhost:80
 test1234

## 폴더 생성
root@manager:/home/docker# mkdir jupyternotebook
root@manager:/home/docker# cd jupyternotebook

#### 마운트 (https://hub.docker.com/r/lionaruc/jupyternotebook 참조)
#### 기본포트 8888 , 환경변수(요즘은 주피터노트북보다 주피터랩을 더 사용 -> yes -> 열 때 랩으로 열림)
#### -v PWD : 현재 디렉토리에 마운트 시키는 것 / 이하 경로 : 원래 지정되어 있는 것 / 9b~ hash 값
#### 이미지 jupyter/datascience-notebook 
docker run --rm -p 8080:8888 -e JUPYTER_ENABLE_LAB=yes -v "$PWD":/home/jupyter/jovyan/work:rw jupyter/datascience-notebook:9b06df75e445

##아래와 같은 에러 발생시 도커머신 재구동   
#Error response from daemon: Get https://registry-1.docker.io/v2/: dial tcp: lookup registry-1.docker.io on 10.0.2.3:53: read udp 10.0.2.15:51254->10.0.2.3:53: i/o timeout.
#docker-machine restart manager

--------------------------------------------------------------------
실습용 코드
docker-cli & Dockerfile로 스프링부트 이미지 빌드하기
https://github.com/moricom2/hello-rest.git

docker-maven-plugin으로 전자정부프레임워크 이미지 빌드하기
https://github.com/moricom2/egovframe-sample.git

CI서버
https://github.com/moricom2/jenkins.git

(추가) 머신의 리소스 수정
docker-machine stop manager
VBoxManage modifyvm manager --cpus 2
VBoxManage modifyvm manager --memory 4096
docker-machine start manager
