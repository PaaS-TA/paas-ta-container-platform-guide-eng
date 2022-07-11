### [Index](https://github.com/PaaS-TA/Guide-eng/blob/master/README.md) > [CP Install](/install-guide/Readme.md) > Service Deployment Portal Installation Guide

<br>

## Table of Contents

1. [Document Outline](#1)  
    1.1. [Purpose](#1.1)  
    1.2. [Range](#1.2)  
    1.3. [System Configuration Diagram](#1.3)  
    1.4. [References](#1.4)  

2. [Prerequisite](#2)  
    2.1. [Firewall Information](#2.1)  
    2.2. [NFS Server Installation](#2.2)    

3. [Container Platform Portal Deployment](#3)  
    3.1. [CRI-O insecure-registry Setting](#3.1)  
    3.2. [Container Platform Portal Deployment](#3.2)  
    3.2.1. [Container Platform Portal Deployment File Download](#3.2.1)  
    3.2.2. [Define Container Platform Portal Variable](#3.2.2)    
    3.2.3. [Execute Container Platform Portal Deployment Script](#3.2.3)    
    3.2.4. [(Refer)Delete Container Platform Portal Resource](#3.2.4)

4. [Configuring Container Platform Portal User Authentication Service](#4)      
    4.1. [Download Container Platform Portal User Authentication Configuration Deployment](#4.1)      
    4.2. [Defining Container Platform Portal User Authentication Configurations](#4.2)      
    4.3. [Execute Container Platform Portal User Authentication Configuration Script](#4.3)          
    4.4. [(Refer) Unconfiguring Container Platform Portal User Authentication](#4.4)    

5. [Container Platform Portal Service Broker](#5)       
    5.1. [Container Platform Portal Service Broker Registration](#5.1)  
    5.2. [Container Platform Portal Service Lookup Settings](#5.2)    
    5.3. [Container Platform User/ Operator Portal Use Guide](#5.3)

6. [Container Platform Portal Reference](#6)  
    6.1. [Create Operator Cluster Role Token](#6.1)    
    6.2. [Precautions for Creating Kubernetes Resources](#6.2)      


## <div id='1'>1. Document Outline
### <div id='1.1'>1.1. Purpose
This document (Container Platform PaaS-TA Service Deployment Portal Installation Guide) describes how to install the Kubernetes Cluster and deploy the Container Platform PaaS-TA Service Deployment Portal.<br>
<br>

### <div id='1.2'>1.2. Range
The installation range was prepared based on the Kubernetes Cluster deployment..

<br>

### <div id='1.3'>1.3. System Configuration Diagram
<p align="center"><img src="images-v1.2/cp-001.png"></p>    

System configuration consists of **Kubernetes Cluster(Master, Worker)** environment and **Network File System(NFS)** ) storage server for data management. It provides containerized middleware environments such as **Harbor** managing container platform portal images and HelmCharts, **Keycloak** managing container platform portal user authentication, and **MariaDB (RDBMS)** managing container platform portal metadata. The total required VM environment is **Master Node VM: 1, Worker Node VM: 1 or more, NFS Server: 1**, and this document is about deploying a container platform portal environment in a Kubernetes cluster. **Network File System (NFS)** is the built-in storage provided by the container platform, and various types of storage can be used depending on the user environment.  

<br>    

### <div id='1.4'>1.4. References
> https://kubernetes.io/ko/docs<br>
> https://goharbor.io/docs<br>
> https://www.keycloak.org/documentation

<br>

## <div id='2'>2. Prerequisite
This installation guide is prepared based on installation in **Ubuntu 18.04** environment.

### <div id='2.1'>2.1. Firewall Information
Set the port that should be opened for the IaaS security group.

- Master Node

| <center>Protocol</center> | <center>Port</center> | <center>Note</center> |  
| :---: | :---: | :--- |  
| TCP | 111 | NFS PortMapper |  
| TCP | 179 | Calio BGP Network |  
| TCP | 2049 | NFS |  
| TCP | 2379-2380 | etcd server client API |  
| TCP | 6443 | Kubernetes API Server |  
| TCP | 10250 | Kubelet API |  
| TCP | 10251 | kube-scheduler |  
| TCP | 10252 | kube-controller-manager |  
| TCP | 10255 | Read-Only Kubelet API |  
| IP-in-IP (Protocol Num 4) || Calico Overlay Network |  

- Worker Node

| <center>Protocol</center> | <center>Port</center> | <center>Note</center> |  
| :---: | :---: | :--- |  
| TCP | 111 | NFS PortMapper |  
| TCP | 179 | Calio BGP network |  
| TCP | 2049 | NFS |  
| TCP | 10250 | Kubelet API |  
| TCP | 10255 | Read-Only Kubelet API |  
| TCP | 30000-32767 | NodePort Services |  
| IP-in-IP (Protocol Num 4) || Calico Overlay Network |  

<br>

### <div id='2.2'>2.2. NFS Server Installation
**NFS Storage Server** installation to be used by container platform portal service must be installed ahead of time.<br>
For NFS Storage Server installation, refer to the guide below.  
> [NFS Server Installation](../nfs-server-install-guide.md)      

<br>

## <div id='3'>3. Container Platform Portal Deployment

### <div id='3.1'>3.1. CRI-O insecure-registry Setting
The container platform portal deployment includes a private repository (Harbor) deployment. To upload images and package files related to the container platform portal to the Private Repository and to set up HTTP connections, install podman in Kubernetes **Master Node, Worker Node**, and set 'insure-registries' in the config file before deployment.

- **:bulb:Master Node, Worker Node needs to set add all**
- **For {K8S_MASTER_NODE_IP} value, enter Kubernetes Master Node Public IP**

#### 1. podman Installation

```
$ sudo apt-get update
$ sudo apt-get install -y podman
```

#### 2. Set'insecure-registries' inside crio.conf  
```
$ sudo vi /etc/crio/crio.conf
```

```
# Add "{K8S_MASTER_NODE_IP}:30002" to the 'insecure_registries' category
...    
insecure_registries = [
 "xx.xxx.xxx.xx:30002"
  ]
...    
```

```
# restart crio
$ sudo systemctl restart crio
```

#### 3. Set insecure in 'registry' category inside registries.conf
```
$ sudo vi /etc/containers/registries.conf
```

```
# Within the registries.conf file, '[[registry]]' needs to be uncommented because it is annotated     
# Add the content below , Set location value as "{K8S_MASTER_NODE_IP}:30002"
...    
[[registry]]
insecure = true
location = "xx.xxx.xxx.xx:30002"
...
```

```
# Restart podman
$ sudo systemctl restart podman
```

<br>

### <div id='3.2'>3.2. Container Platform Portal Deployment

#### <div id='3.2.1'>3.2.1. Container Platform Portal Deployment File Download
Download the container platform portal Deployment file and locate it in the path below for container platform portal deployment.<br>
:bulb: Process the content at Kubernetes **Master Node**.

+ Container Platform Portal Deployment File Download :
   [paas-ta-container-platform-portal-deployment.tar.gz](https://nextcloud.paas-ta.org/index.php/s/wYJ3wim3WCxG7Ed/download)

```
# Create Deployment File Download Path
$ mkdir -p ~/workspace/container-platform
$ cd ~/workspace/container-platform

# Deployment File Download and Check File Path
$ wget --content-disposition https://nextcloud.paas-ta.org/index.php/s/wYJ3wim3WCxG7Ed/download

$ ls ~/workspace/container-platform
  paas-ta-container-platform-portal-deployment.tar.gz

#  Unzip Deployment File
$ tar -xvf paas-ta-container-platform-portal-deployment.tar.gz
```

- Configure Deployment File Directory
```
├── script          # Container Platform Portal Deployment Related Variable and Script File Location
├── images          # Container Platform Portal Image File Location
├── charts          # Container Platform Portal Helm Charts File Location
├── values_orig     # Container Platform Portal Helm Charts values.yaml File Location
└── keycloak_orig   # Location of files related to Keycloak deployment for container platform portal user authentication management
```

<br>

#### <div id='3.2.2'>3.2.2. Define Container Platform Portal Variable
Defining variable values is necessary before deploying the container platform portal. Set the variable by checking the information required for deployment.

:bulb: Keycloak's default deployment method is **HTTP**. If you want to set **HTPS** through a certificate, refer to the guide below for pre-processing.
> [Keycloak TLS Setting](paas-ta-container-platform-portal-deployment-keycloak-tls-setting-guide-v1.2.md#2-keycloak-tls-설정)       

<br>

```
$ cd ~/workspace/container-platform/paas-ta-container-platform-portal-deployment/script
$ vi container-platform-portal-vars.sh
```

```                                                     
# COMMON VARIABLE
K8S_MASTER_NODE_IP="{k8s master node public ip}"             # Kubernetes Master Node Public IP
K8S_AUTH_BEARER_TOKEN="{k8s auth bearer token}"              # Kubernetes Authorization Bearer Token
NFS_SERVER_IP="{nfs server ip}"                              # NFS Server IP
PROVIDER_TYPE="{container platform portal provider type}"    # Container Platform Portal Provider Type (Please enter 'standalone' or 'service')
....    
```
```    
# Example
K8S_MASTER_NODE_IP="xx.xxx.xxx.xx"                 
K8S_AUTH_BEARER_TOKEN="qY3k2xaZpNbw3AJxxxxx...."                 
NFS_SERVER_IP="xx.xxx.xxx.xx"                                  
PROVIDER_TYPE="service"           
```

- **K8S_MASTER_NODE_IP** <br>Enter `Kubernetes Master Node Public IP<br><br>
- **K8S_AUTH_BEARER_TOKEN** <br>Enter Kubernetes Bearer Token<br>
   + Refer to [[6.1. Create Operator Cluster Role Token]](#6.1) to create and enter Token value<br><br>
- **NFS_SERVER_IP** <br>Enter NFS Server Private IP<br>
   + Enter the NFS Server Private IP installed through [[NFS Server Installation](../nfs-server-install-guide.md)] guide<br><br>
- **PROVIDER_TYPE** <br>Enter Container Platform Portal Providing Type <br>
   + This guide is a portal PaaS-TA service deployment installation guide that requires **'service'** value

<br>    

#### <div id='3.2.3'>3.2.3. 컨테이너 플랫폼 포털 배포 스크립트 실행
컨테이너 플랫폼 포털 배포를 위한 배포 스크립트를 실행한다.

```
$ chmod +x deploy-container-platform-portal.sh
$ ./deploy-container-platform-portal.sh
```
<br>

컨테이너 플랫폼 포털 관련 리소스가 정상적으로 배포되었는지 확인한다.<br>
리소스 Pod의 경우 Node에 바인딩 및 컨테이너 생성 후 Running 상태로 전환되기까지 몇 초가 소요된다.

- **NFS 리소스 조회**
>`$ kubectl get all -n nfs-storageclass`  
```
$ kubectl get all -n nfs-storageclass
NAME                                       READY   STATUS    RESTARTS   AGE
pod/nfs-pod-provisioner-7fff84f48f-ltd2w   1/1     Running   0          3m24s

NAME                                  READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nfs-pod-provisioner   1/1     1            1           3m24s

NAME                                             DESIRED   CURRENT   READY   AGE
replicaset.apps/nfs-pod-provisioner-7fff84f48f   1         1         1       3m24s
```

- **Harbor 리소스 조회**
>`$ kubectl get all -n harbor`   
```
$ kubectl get all -n harbor
NAME                                                                  READY   STATUS    RESTARTS   AGE
pod/paas-ta-container-platform-harbor-chartmuseum-859f844877-npzw9    1/1     Running   0          3m59s
pod/paas-ta-container-platform-harbor-core-5f8cbdfc4b-vnbh5           1/1     Running   0          3m59s
pod/paas-ta-container-platform-harbor-database-0                      1/1     Running   0          3m59s
pod/paas-ta-container-platform-harbor-jobservice-c8cb6fd44-gc5vn      1/1     Running   0          3m59s
pod/paas-ta-container-platform-harbor-nginx-755cf6876f-znmnn          1/1     Running   0          3m59s
pod/paas-ta-container-platform-harbor-notary-server-9bb4d9774-rjd72   1/1     Running   0          3m59s
pod/paas-ta-container-platform-harbor-notary-signer-7fd8996c68jfb7v   1/1     Running   0          3m59s
pod/paas-ta-container-platform-harbor-portal-c79c78f5b-4w5qz          1/1     Running   0          3m59s
pod/paas-ta-container-platform-harbor-redis-0                         1/1     Running   0          3m59s
pod/paas-ta-container-platform-harbor-registry-5d478577d7-hv76l       2/2     Running   0          3m59s
pod/paas-ta-container-platform-harbor-trivy-0                         1/1     Running   0          3m59s

NAME                                                      TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)                       AGE
service/harbor                                            NodePort    10.233.4.37     <none>        80:30002/TCP,4443:30004/TCP   3m59s
service/paas-ta-container-platform-harbor-chartmuseum     ClusterIP   10.233.1.163    <none>        80/TCP                        3m59s
service/paas-ta-container-platform-harbor-core            ClusterIP   10.233.4.62     <none>        80/TCP                        3m59s
service/paas-ta-container-platform-harbor-database        ClusterIP   10.233.45.86    <none>        5432/TCP                      3m59s
service/paas-ta-container-platform-harbor-jobservice      ClusterIP   10.233.54.157   <none>        80/TCP                        3m59s
service/paas-ta-container-platform-harbor-notary-server   ClusterIP   10.233.11.94    <none>        4443/TCP                      3m59s
service/paas-ta-container-platform-harbor-notary-signer   ClusterIP   10.233.56.213   <none>        7899/TCP                      3m59s
service/paas-ta-container-platform-harbor-portal          ClusterIP   10.233.27.172   <none>        80/TCP                        3m59s
service/paas-ta-container-platform-harbor-redis           ClusterIP   10.233.44.10    <none>        6379/TCP                      3m59s
service/paas-ta-container-platform-harbor-registry        ClusterIP   10.233.54.76    <none>        5000/TCP,8080/TCP             3m59s
service/paas-ta-container-platform-harbor-trivy           ClusterIP   10.233.3.176    <none>        8080/TCP                      3m59s

NAME                                                              READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/paas-ta-container-platform-harbor-chartmuseum     1/1     1            1           3m59s
deployment.apps/paas-ta-container-platform-harbor-core            1/1     1            1           3m59s
deployment.apps/paas-ta-container-platform-harbor-jobservice      1/1     1            1           3m59s
deployment.apps/paas-ta-container-platform-harbor-nginx           1/1     1            1           3m59s
deployment.apps/paas-ta-container-platform-harbor-notary-server   1/1     1            1           3m59s
deployment.apps/paas-ta-container-platform-harbor-notary-signer   1/1     1            1           3m59s
deployment.apps/paas-ta-container-platform-harbor-portal          1/1     1            1           3m59s
deployment.apps/paas-ta-container-platform-harbor-registry        1/1     1            1           3m59s

NAME                                                                         DESIRED   CURRENT   READY   AGE
replicaset.apps/paas-ta-container-platform-harbor-chartmuseum-859f844877     1         1         1       3m59s
replicaset.apps/paas-ta-container-platform-harbor-core-5f8cbdfc4b            1         1         1       3m59s
replicaset.apps/paas-ta-container-platform-harbor-jobservice-c8cb6fd44       1         1         1       3m59s
replicaset.apps/paas-ta-container-platform-harbor-nginx-755cf6876f           1         1         1       3m59s
replicaset.apps/paas-ta-container-platform-harbor-notary-server-9bb4d9774    1         1         1       3m59s
replicaset.apps/paas-ta-container-platform-harbor-notary-signer-7fd8996c68   1         1         1       3m59s
replicaset.apps/paas-ta-container-platform-harbor-portal-c79c78f5b           1         1         1       3m59s
replicaset.apps/paas-ta-container-platform-harbor-registry-5d478577d7        1         1         1       3m59s

NAME                                                          READY   AGE
statefulset.apps/paas-ta-container-platform-harbor-database   1/1     3m59s
statefulset.apps/paas-ta-container-platform-harbor-redis      1/1     3m59s
statefulset.apps/paas-ta-container-platform-harbor-trivy      1/1     3m59s
```

- **MariaDB 리소스 조회**
>`$ kubectl get all -n mariadb`  
```
$ kubectl get all -n mariadb
NAME                                       READY   STATUS    RESTARTS   AGE
pod/paas-ta-container-platform-mariadb-0   1/1     Running   0          2m13s

NAME                                         TYPE       CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
service/paas-ta-container-platform-mariadb   NodePort   10.233.24.16   <none>        3306:31306/TCP   2m13s

NAME                                                  READY   AGE
statefulset.apps/paas-ta-container-platform-mariadb   1/1     2m13s
```    

- **Keycloak 리소스 조회**
>`$ kubectl get all -n keycloak`  
```
$ kubectl get all -n keycloak
NAME                            READY   STATUS    RESTARTS   AGE
pod/keycloak-5bdb65fcb5-25cmn   1/1     Running   0          3m10s
pod/keycloak-5bdb65fcb5-ndxtm   1/1     Running   0          3m10s

NAME                       TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
service/keycloak           NodePort    10.233.56.97   <none>        8080:32710/TCP   3m10s
service/keycloak-cluster   ClusterIP   None           <none>        8080/TCP         3m10s

NAME                       READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/keycloak   2/2     2            2           3m10s

NAME                                  DESIRED   CURRENT   READY   AGE
replicaset.apps/keycloak-5bdb65fcb5   2         2         2       3m10s
```

- **컨테이너 플랫폼 포털 리소스 조회**
>`$ kubectl get all -n paas-ta-container-platform-portal`   
```
$ kubectl get all -n paas-ta-container-platform-portal
NAME                                                                  READY   STATUS    RESTARTS   AGE
pod/container-platform-admin-service-broker-deployment-54cdcc7qzxxn   1/1     Running   0          3m23s
pod/container-platform-api-deployment-64d487c88-2c9zw                 1/1     Running   0          3m26s
pod/container-platform-common-api-deployment-645c8486dd-lf94j         1/1     Running   0          3m25s
pod/container-platform-user-service-broker-deployment-547fdb7c45mkd   1/1     Running   0          3m23s
pod/container-platform-webadmin-deployment-587645b5c-bvpfr            1/1     Running   0          3m25s
pod/container-platform-webuser-deployment-58b6b79669-jg8vf            1/1     Running   0          3m24s

NAME                                                      TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
service/container-platform-admin-service-broker-service   NodePort   10.233.1.48     <none>        3330:32704/TCP   3m23s
service/container-platform-api-service                    NodePort   10.233.47.173   <none>        3333:32701/TCP   3m26s
service/container-platform-common-api-service             NodePort   10.233.46.171   <none>        3334:32700/TCP   3m25s
service/container-platform-user-service-broker-service    NodePort   10.233.54.150   <none>        3331:32705/TCP   3m23s
service/container-platform-webadmin-service               NodePort   10.233.57.102   <none>        8090:32703/TCP   3m25s
service/container-platform-webuser-service                NodePort   10.233.0.236    <none>        8091:32702/TCP   3m24s

NAME                                                                 READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/container-platform-admin-service-broker-deployment   1/1     1            1           3m23s
deployment.apps/container-platform-api-deployment                    1/1     1            1           3m26s
deployment.apps/container-platform-common-api-deployment             1/1     1            1           3m25s
deployment.apps/container-platform-user-service-broker-deployment    1/1     1            1           3m23s
deployment.apps/container-platform-webadmin-deployment               1/1     1            1           3m25s
deployment.apps/container-platform-webuser-deployment                1/1     1            1           3m24s

NAME                                                                            DESIRED   CURRENT   READY   AGE
replicaset.apps/container-platform-admin-service-broker-deployment-54cdcc7b76   1         1         1       3m23s
replicaset.apps/container-platform-api-deployment-64d487c88                     1         1         1       3m26s
replicaset.apps/container-platform-common-api-deployment-645c8486dd             1         1         1       3m25s
replicaset.apps/container-platform-user-service-broker-deployment-547fdb7cb5    1         1         1       3m23s
replicaset.apps/container-platform-webadmin-deployment-587645b5c                1         1         1       3m25s
replicaset.apps/container-platform-webuser-deployment-58b6b79669                1         1         1       3m24s
```    

<br>

#### <div id='3.2.4'>3.2.4. (참조) 컨테이너 플랫폼 포털 리소스 삭제
배포된 컨테이너 플랫폼 포털 리소스의 삭제를 원하는 경우 아래 스크립트를 실행한다.<br>
:loudspeaker: (주의) 컨테이너 플랫폼 포털이 운영되는 상태에서 해당 스크립트 실행 시, **운영에 필요한 리소스가 모두 삭제**되므로 주의가 필요하다.<br>

```
$ cd ~/workspace/container-platform/paas-ta-container-platform-portal-deployment/script
$ chmod +x uninstall-container-platform-portal.sh
$ ./uninstall-container-platform-portal.sh
```
```    
Are you sure you want to delete the container platform portal? <y/n> y
....
release "paas-ta-container-platform-harbor" uninstalled
release "paas-ta-container-platform-mariadb" uninstalled
release "paas-ta-container-platform-keycloak" uninstalled
release "paas-ta-container-platform-nfs-storageclass" uninstalled
release "paas-ta-container-platform-api" uninstalled
release "paas-ta-container-platform-common-api" uninstalled
release "paas-ta-container-platform-webadmin" uninstalled
release "paas-ta-container-platform-webuser" uninstalled
release "paas-ta-container-platform-admin-service-broker" uninstalled
release "paas-ta-container-platform-user-service-broker" uninstalled
namespace "harbor" deleted
namespace "mariadb" deleted
namespace "keycloak" deleted
namespace "nfs-storageclass" deleted
namespace "paas-ta-container-platform-portal" deleted
"paas-ta-container-platform-repository" has been removed from your repositories
Uninstalled plugin: cm-push
....    
```

<br>   

## <div id='4'>4. 컨테이너 플랫폼 포털 사용자 인증 서비스 구성
컨테이너 플랫폼 포털 사용자 인증은 Keycloak 서비스를 통해 관리된다. PaaS-TA 포털의 사용자 인증 서비스 UAA의 사용자 계정으로 컨테이너 플랫폼 포털 접속을 위해
UAA 서비스를 ID 제공자(Identity Provider)로, Keycloak 서비스를 서비스 제공자(Service Provider)로 구성하는 단계가 필요하다.

#### <div id='4.1'>4.1. 컨테이너 플랫폼 포털 사용자 인증 구성 Deployment 다운로드
UAA 서비스와 Keycloak 서비스 인증 구성을 위한 Deployment 파일을 다운로드 받아 아래 경로로 위치시킨다.<br>
:bulb: 해당 내용은 PaaS-TA 포털이 설치된 **BOSH Inception**에서 진행한다.

+ 컨테이너 플랫폼 포털 사용자 인증 구성 Deployment 다운로드 :  
   [paas-ta-container-platform-saml-deployment.tar.gz](https://nextcloud.paas-ta.org/index.php/s/iJYjroasEA9BJgs/download)  

```
# Deployment 파일 다운로드 경로 생성
$ mkdir -p ~/workspace/container-platform
$ cd ~/workspace/container-platform

# Deployment 파일 다운로드 및 파일 경로 확인
$ wget --content-disposition https://nextcloud.paas-ta.org/index.php/s/iJYjroasEA9BJgs/download

$ ls ~/workspace/container-platform
  paas-ta-container-platform-saml-deployment.tar.gz

# Deployment 파일 압축 해제
$ tar -xvf paas-ta-container-platform-saml-deployment.tar.gz
```
<br>

#### <div id='4.2'>4.2. 컨테이너 플랫폼 포털 사용자 인증 구성 변수 정의
UAA 서비스와 Keycloak 서비스 인증 구성을 위한 변수 값 정의가 필요하다. 구성에 필요한 정보를 확인하여 변수를 설정한다.

:bulb: **Keycloak TLS HTTPS** 설정이 적용된 경우, Keycloak URL 변수 값 변경이 필요하다. <br>
아래 가이드를 참조하여 변수 값을 변경한다.
> [(서비스형 배포) 사용자 인증 서비스 구성 변경](paas-ta-container-platform-portal-deployment-keycloak-tls-setting-guide-v1.2.md#3-서비스형-배포-사용자-인증-서비스-구성-변경)       

<br>

```
$ cd ~/workspace/container-platform/paas-ta-container-platform-saml-deployment
$ vi container-platform-saml-vars.sh
```

```                                                     
# COMMON VARIABLE
PAASTA_SYSTEM_DOMAIN="xx.xxx.xx.xxx.nip.io"                       # PaaS-TA System Domain
K8S_MASTER_NODE_IP="xx.xxx.xx.xxx"                                # Kubernetes Master Node Public Ip
UAA_CLIENT_ADMIN_ID="admin"                                       # UAA Admin Client ID (e.g. admin)
UAA_CLIENT_ADMIN_SECRET="admin-secret"                            # UAA Admin Client Secret (e.g. admin-secret)
....    
```


- **PAASTA_SYSTEM_DOMAIN** <br> PaaS-TA 배포 시 지정했던 System Domain 명 입력<br><br>
- **K8S_MASTER_NODE_IP** <br>Kubernetes Master Node Public IP 입력<br><br>
- **UAA_CLIENT_ADMIN_ID** <br>UAAC Admin Client Admin ID 입력 (기본 값 : admin)<br><br>
- **UAA_CLIENT_ADMIN_SECRET** <br>UAAC Admin Client에 접근하기 위한 Secret 변수 (기본 값 : admin-secret)<br><br>

<br>

#### <div id='4.3'>4.3. 컨테이너 플랫폼 포털 사용자 인증 구성 스크립트 실행
UAA 서비스와 Keycloak 서비스 인증 구성을 위한 스크립트를 실행한다.

```
$ chmod +x create-service-provider.sh
$ ./create-service-provider.sh
```

<br>

구성이 정상적으로 처리되었는지 확인한다. (**RESPONSE BODY 내 결과 확인**)
- UAAC Service Providers 조회   
>`$ uaac curl /saml/service-providers --insecure`     
```    
$ uaac curl /saml/service-providers --insecure
GET https://uaa.xx.xxx.xxx.xx.nip.io/saml/service-providers

200 OK
RESPONSE HEADERS:
  Cache-Control: no-cache, no-store, max-age=0, must-revalidate
  Content-Type: application/json
  Date: Tue, 14 Dec 2021 02:08:06 GMT
  Expires: 0
  Pragma: no-cache
  Strict-Transport-Security: max-age=31536000 ; includeSubDomains
  X-Content-Type-Options: nosniff
  X-Frame-Options: DENY
  X-Vcap-Request-Id: e6934abf-d9b8-41ae-7e3f-bebfa688d198
  X-Xss-Protection: 1; mode=block
  Content-Length: 1768
  Connection: close
RESPONSE BODY:
[
  {
    "config": "{\"metaDataLocation\": .... }",
    "id": "0679dca3-0461-4af9-b513-7c114b6f9110",
    "entityId": "http://xx.xxx.xxx.xx:32710/auth/realms/container-platform-realm",
    "name": "paas-ta-container-platform-saml-sp",
    "version": 0,
    "created": 1639447555314,
    "lastModified": 1639447555314,
    "active": true,
    "identityZoneId": "uaa"
  }
....    
]
```    

<br>

#### <div id='4.4'>4.4. (참조) 컨테이너 플랫폼 포털 사용자 인증 구성 해제
UAA 서비스와 Keycloak 서비스 인증 구성 해제를 원하는 경우 아래 스크립트를 실행한다.<br>
:loudspeaker: (주의) 컨테이너 플랫폼 포털이 운영되는 상태에서 해당 스크립트 실행 시, 사용자 인증 구성이 불가하므로 주의가 필요하다.<br>


##### 해제할 Service Provider ID 조회
UAAC Service Providers 조회 후 **RESPONSE BODY** 결과 내 아래 조건을 가진 **Service Provider ID**를 조회한다.
- `entityId : http://{K8S_MASTER_NODE_IP}:32710/auth/realms/container-platform-realm` <br>
- `name : paas-ta-container-platform-saml-sp` <br>

```  
$ uaac curl /saml/service-providers --insecure

....
RESPONSE BODY:
[
  {
    "config": "{\"metaDataLocation\": .... }",
    "id": "0679dca3-0461-4af9-b513-7c114b6f9110",   # 해제할 Service Provider ID
    "entityId": "http://xx.xxx.xxx.xx:32710/auth/realms/container-platform-realm",
    "name": "paas-ta-container-platform-saml-sp",
    "version": 0,
    "created": 1639447555314,
    "lastModified": 1639447555314,
    "active": true,
    "identityZoneId": "uaa"
  }
....    
]
```    

<br>

해제할 **Service Provider ID** 조회 후 인증 구성 해제 스크립트를 실행한다.

```
$ cd ~/workspace/container-platform/paas-ta-container-platform-saml-deployment
$ chmod +x uninstall-service-provider.sh
$ ./uninstall-service-provider.sh {Service_Provider_ID}
```

```    
$ ./uninstall-service-provider.sh 0679dca3-0461-4af9-b513-7c114b6f9110
....  
Are you sure you want to delete this service provider? <y/n> y
DELETE https://uaa.13.125.147.203.nip.io/saml/service-providers/0679dca3-0461-4af9-b513-7c114b6f9110
....    
```

<br>

## <div id='5'>5. 컨테이너 플랫폼 포털 서비스 브로커
컨테이너 플랫폼 PaaS-TA 서비스 형 포털로 설치하는 경우 CF와 Kubernetes에 배포된 컨테이너 플랫폼 포털 서비스 연동을 위해서 브로커를 등록해 주어야 한다.
PaaS-TA 운영자 포털을 통해 서비스를 등록하고 공개하면, PaaS-TA 사용자 포털을 통해 서비스를 신청하여 사용할 수 있다.

### <div id='5.1'>5.1. 컨테이너 플랫폼 포털 서비스 브로커 등록
서비스 브로커 등록 시 개방형 클라우드 플랫폼에서 서비스 브로커를 등록할 수 있는 사용자로 로그인이 되어있어야 한다.

##### 서비스 브로커 목록을 확인한다.
>`$ cf service-brokers`
```
$ cf service-brokers
Getting service brokers as admin...

name   url
No service brokers found
```


##### 컨테이너 플랫폼 포털 서비스 브로커를 등록한다.
>`$ cf create-service-broker {서비스팩 이름} {서비스팩 사용자ID} {서비스팩 사용자비밀번호} http://{서비스팩 URL}`

서비스팩 이름 : 서비스 팩 관리를 위해 개방형 클라우드 플랫폼에서 보여지는 명칭<br>
서비스팩 사용자 ID/비밀번호 : 서비스팩에 접근할 수 있는 사용자 ID/비밀번호<br>
서비스팩 URL : 서비스팩이 제공하는 API를 사용할 수 있는 URL<br>


###### 컨테이너 플랫폼 운영자 포털 서비스 브로커 등록
>`$ cf create-service-broker container-platform-admin-portal-service-broker admin cloudfoundry http://{K8S_MASTER_NODE_IP}:32704`   
###### 컨테이너 플랫폼 사용자 포털 서비스 브로커 등록     
>`$ cf create-service-broker container-platform-user-portal-service-broker admin cloudfoundry http://{K8S_MASTER_NODE_IP}:32705`

```
$ cf create-service-broker container-platform-admin-portal-service-broker admin cloudfoundry http://xx.xxx.xxx.xx:32704
Creating service broker container-platform-admin-portal-service-broker as admin...
OK

$ cf create-service-broker container-platform-user-portal-service-broker admin cloudfoundry http://xx.xxx.xxx.xx:32705
Creating service broker container-platform-user-portal-service-broker as admin...
OK
```    


##### 등록된 컨테이너 플랫폼 포털 서비스 브로커를 확인한다.
>`$ cf service-brokers`
```
$ cf service-brokers
Getting service brokers as admin...
name                                             url
container-platform-admin-portal-service-broker   http://xx.xxx.xxx.xx:32704
container-platform-user-portal-service-broker    http://xx.xxx.xxx.xx:32705
```


##### 접근 가능한 서비스 목록을 확인한다.
>`$ cf service-access`     
```
$ cf service-access
Getting service access as admin...

broker: container-platform-admin-portal-service-broker
   offering                                         plan       access   orgs
   container-platform-admin-portal-service-broker   Advenced   none

broker: container-platform-user-portal-service-broker
   offering                                        plan       access   orgs
   container-platform-user-portal-service-broker   Advenced   none
   container-platform-user-portal-service-broker   Micro      none
   container-platform-user-portal-service-broker   Small      none
```


##### 특정 조직에 해당 서비스 접근 허용을 할당한다.

###### 컨테이너 플랫폼 운영자 포털 서비스 접근 허용 할당  
>`$ cf enable-service-access container-platform-admin-portal-service-broker`   
###### 컨테이너 플랫폼 사용자 포털 서비스 접근 허용 할당     
>`$ cf enable-service-access container-platform-user-portal-service-broker`

```
$ cf enable-service-access container-platform-admin-portal-service-broker
Enabling access to all plans of service offering container-platform-admin-portal-service-broker for all orgs as admin...
OK

$ cf enable-service-access container-platform-user-portal-service-broker
Enabling access to all plans of service offering container-platform-user-portal-service-broker for all orgs as admin...
OK
```


##### 접근 가능한 서비스 목록을 확인한다.
>`$ cf service-access`

```
$ cf service-access
Getting service access as admin...

broker: container-platform-admin-portal-service-broker
   offering                                         plan       access   orgs
   container-platform-admin-portal-service-broker   Advenced   all

broker: container-platform-user-portal-service-broker
   offering                                        plan       access   orgs
   container-platform-user-portal-service-broker   Advenced   all
   container-platform-user-portal-service-broker   Micro      all
   container-platform-user-portal-service-broker   Small      all
```

<br>

### <div id='5.2'>5.2. 컨테이너 플랫폼 포털 서비스 조회 설정
해당 설정은 PaaS-TA 포털에서 컨테이너 플랫폼 포털 서비스를 조회하고 신청할 수 있도록 하기 위한 설정이다.

##### PaaS-TA 운영자 포털에 접속한다.
![image 007]


##### 메뉴 [운영관리]-[카탈로그] 에서 앱서비스 탭 안에 Container Platform Admin Portal, Container Platform User Portal 서비스를 선택하여 설정을 변경한다.
![image 008]

##### Container Platform Admin Portal 서비스를 선택하여 아래와 같이 설정 변경 후 저장한다.
>`'서비스' 항목 : 'container-platform-admin-portal-service-broker' 로 선택` <br>
>`'공개' 항목 : 'Y' 로 체크`

![image 009]

##### Container Platform User Portal 서비스를 선택하여 아래와 같이 설정 변경 후 저장한다.
>`'서비스' 항목 : 'container-platform-user-portal-service-broker' 로 선택` <br>
>`'공개' 항목 : 'Y' 로 체크`

![image 010]    

<br>

#### :bulb: 컨테이너 플랫폼 운영자 포털 서비스 신청 시 유의사항
- 컨테이너 플랫폼 운영자 포털 서비스의 경우 조직명 **'portal'** 조직에서만 신청 가능하다.
- 컨테이너 플랫폼 운영자 포털 서비스는 전체 조직 내 한 조직에서만 신청 가능하며 이는 PaaS-TA Portal 배포 시 디폴트로 생성되는 조직 **'portal'** 로 지정하였다.
- 서비스 신청 시 조직 **'portal'** 이 없는 경우 **'portal'** 명으로 조직 생성 후 서비스 신청이 필요하다.

<br>

>`컨테이너 플랫폼 운영자 포털 서비스 신청 시 조직 명 'portal' 확인 후 신청 필요`     

![image 011]       

<br>

### <div id='5.3'/>5.3. 컨테이너 플랫폼 사용자/운영자 포털 사용 가이드
- 컨테이너 플랫폼 포털 사용방법은 아래 사용가이드를 참고한다.  
  + [컨테이너 플랫폼 운영자 포털 사용 가이드](../../use-guide/portal/container-platform-admin-portal-guide.md)    
  + [컨테이너 플랫폼 사용자 포털 사용 가이드](../../use-guide/portal/container-platform-user-portal-guide.md)


<br>

## <div id='6'>6. 컨네이너 플랫폼 포털 참고

### <div id='6.1'>6.1. 운영자 Cluster Role Token 생성
Cluster Role을 가진 운영자의 Service Account를 생성하고 해당 Service Account의 Token 값을 획득한다.<br>
획득한 Token 값은 컨테이너 플랫폼 포털 배포 시 사용된다.

- Service Account를 생성한다.
```
## {SERVICE_ACCOUNT} : 생성할 Service Account 명

$ kubectl create serviceaccount {SERVICE_ACCOUNT} -n kube-system
(ex. kubectl create serviceaccount k8sadmin -n kube-system)
```

- 생성한 Service Account와 kubernetes에서 제공하는 ClusterRole 'cluster-admin'을 바인딩한다.
```
$ kubectl create clusterrolebinding {SERVICE_ACCOUNT} --clusterrole=cluster-admin --serviceaccount=kube-system:{SERVICE_ACCOUNT}
(ex. kubectl create clusterrolebinding k8sadmin --clusterrole=cluster-admin --serviceaccount=kube-system:k8sadmin)
```

- Service Account의 Mountable secrets 값을 확인한다.
```
$ kubectl describe serviceaccount {SERVICE_ACCOUNT} -n kube-system
(ex. kubectl describe serviceaccount k8sadmin -n kube-system)

...

Mountable secrets:   k8sadmin-token-xxxx
```

- Service Account의 Token을 획득한다.

```
## {SECRET_NAME} : Mountable secrets 값 입력

$ kubectl describe secret {SECRET_NAME} -n kube-system | grep -E '^token' | cut -f2 -d':' | tr -d " "
```

<br>

### <div id='6.2'>6.2. Kubernetes 리소스 생성 시 주의사항

컨테이너 플랫폼 이용 중 리소스 생성 시 다음과 같은 prefix를 사용하지 않도록 주의한다.

|Resource 명|생성 시 제외해야 할 prefix|
|---|---|
|전체 Resource|kube*|
|Namespace|all|
||kubernetes-dashboard|
||paas-ta-container-platform-temp-namespace|
|Role|paas-ta-container-platform-init-role|
||paas-ta-container-platform-admin-role|
|ResourceQuota|paas-ta-container-platform-low-rq|
||paas-ta-container-platform-medium-rq|
||paas-ta-container-platform-high-rq|
|LimitRanges|paas-ta-container-platform-low-limit-range|
||paas-ta-container-platform-medium-limit-range|
||paas-ta-container-platform-high-limit-range|
|Pod|nodes|
||resources|

<br>

### [Index](https://github.com/PaaS-TA/Guide-eng/blob/master/README.md) > [CP Install](/install-guide/Readme.md) > 서비스형 배포 포털 설치 가이드

[image 001]:images-v1.2/cp-001.png
[image 002]:images-v1.2/cp-002.png
[image 003]:images-v1.2/cp-003.png
[image 004]:images-v1.2/cp-004.png
[image 005]:images-v1.2/cp-005.png
[image 006]:images-v1.2/cp-006.png
[image 007]:images-v1.2/cp-007.png
[image 008]:images-v1.2/cp-008.png
[image 009]:images-v1.2/cp-009.png
[image 010]:images-v1.2/cp-010.png
[image 011]:images-v1.2/cp-011.png
