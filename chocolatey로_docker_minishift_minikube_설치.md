### chocolatey 활용

#### chocolatey 로 docker 설치 (virtualbox 설치)

1) cmd.exe (관리자 권한으로 실행)
```
systeminfo
```
> 다음과 같은 출력이 확인되어야 함
```
Hyper-V Requirements:
    VM Monitor Mode Extensions: Yes
    Virtualization Enabled In Firmware: Yes
    Second Level Address Translation: Yes
    Data Execution Prevention Available: Yes
```

2) chocolatey 설치

> https://chocolatey.org/install#non-administrative-install 참조<br>
> 위 링크에 있는 내용 1개 복사해서 실행<br>
> 만약 -ssl/tls 보안 채널을 만들 수 없습니다 에러 발생하면, 아래 링크 참조<br>
```
powershell 관리자로 실행 (윈도우에선 powershell로만 실행됨)
https://chocolatey.org/blog/remove-support-for-old-tls-versions
위 링크에 있는 내용 3개 복사해서 실행
```

> choco로 설치한 패키지 목록 보기
```
choco list —local-only
```

`지원되는 하이파바이저 : VirtualBox(권장), Hyper-V`

3) virtualbox 설치
```
choco install -y virtualbox virtualbox.extensionpack
```

4) Docker 컴포넌트 설치
```
choco install -y docker-cli docker-machine docker-compose
```

5) docker Server 생성 - vm 생성 (생성하는 vm의 이름은 manager)
```
docker-machine create --driver virtualbox manager
```

6) vm 목록 확인
```
docker-machine ls
```

7) vm 시작/중지
```
docker-machine start/stop manager
```

8) 도커 클라이언트 환경 셋팅
```
docker-machine env manager
```

> 다음과 같은 출력라인 확인후 copy&paste
```
REM Run this command to configure your shell:
REM @FOR /f "tokens=*" %i IN ('"C:\ProgramData\chocolatey\lib\docker-machine\bin\docker-machine.exe" env manager') DO @%i  
-> (예시)
```

> C:\Windows\system32> 즉, 이곳에 아래 문자열을 복사함
```
& "C:\ProgramData\chocolatey\lib\docker-machine\bin\docker-machine.exe" env manager | Invoke-Expression
```

9) docker 클라이언트/서버 버전 확인
```
docker version
```


#### chocolatey 로 minishift 설치

1) https://www.openshift.org/minishift/ release 페이지에서 최신버전 다운 후 실행 
2) C:\minishift 폴더를 만들고 이곳에 위에서 받은 압축 파일의 내용물 이동
3) cmder
```
cd C:\minishift
minishift.exe start --vm-driver virtualbox
 => virtualbox를 하이퍼바이저 (oc 도 함께 설치됨)
```
4) 확인
```
C:\minishift>minishift ip
192.168.99.100

C:\minishift>minishift status
Running

*참고 : https://javaworld.co.kr/103
```

#### chocolatey 로 minikube 설치

1) minkube 설치 
```
choco install minikube kubernetes-cli
```

2) minkube 구동 (minikube start —driver=<driver_name>)
```  
  ( <driver_name> 은 하이퍼바이저 이름 ) 
minikube start —driver=virtualobx
  ==> 엄청 오래걸림......................
```

3) 클러스터의 상태를 확인
```
minikube status
=> 결과는 아래와 유사해야 함 
- host: Running
- kubelet: Running
- apiserver: Running
- kubeconfig: Configured

*참고 :　https://kubernetes.io/ko/docs/tasks/tools/install-minikube/
        https://kubernetes.io/docs/setup/learning-environment/minikube/
```

#### docker 쉘 실행 
1) docker machine 접속
```
docker-machine ssh manager
```

2) root 계정
```
sudo su
```
