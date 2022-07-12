### [Index](https://github.com/PaaS-TA/Guide-eng/blob/master/README.md) > [CP Install](https://github.com/PaaS-TA/paas-ta-container-platform-guide-eng/tree/master/install-guide/Readme.md) > Edge Installation Guide

<br>

## Table of Contents

1. [Document Outline](#1)  
  1.1. [Purpose](#1.1)  
  1.2. [Range](#1.2)  
  1.3. [System Configuration Diagram](#1.3)  
  1.4. [References](#1.4)  

2. [KubeEdge Installation](#2)  
  2.1. [Prerequisite](#2.1)  
  2.2. [Kubernetes Native Cluster Deployment](#2.2)  
  2.3. [KubeEdge keadm Installation](#2.3)  
  2.4. [KubeEdge CloudCore Installation](#2.4)  
  2.5. [KubeEdge EdgeCore Installation](#2.5)  
  2.6. [Change DaemonSet Settings](#2.6)  
  2.7. [Enable the kubectl logs feature](#2.7)  
  2.8. [EdgeMesh Deployment](#2.8)  
  2.9. [KubeEdge Installation Check](#2.9)  

3. [KubeEdge Reset (Refer)](#3)  

4. [Create Container Platform Operator and Acquire Token](#4)  
  4.1. [Create Cluster Role Operator and Acquire Token](#4.1)  
  4.2. [Namespace User Token Acquire](#4.2)  

5. [Precautions When Creating Resource](#5)

<br>

## <div id='1'> 1. Document Outline

### <div id='1.1'> 1.1. Purpose
This document (KubeEdge Installation Guide) describes how to install KubeEdge to upgrade the open PaaS platform and install a container platform deployment to Open PaaS based on a developer support environment.

Starting with PaaS-TA 5.5 version, it supports exclusive deployment based on KubeEdge. To install it based on an existing Container service, refer to the PaaS-TA 5.0 or lower version of the document. 

<br>

### <div id='1.2'> 1.2. Range
The installation range was prepared based on the basic installation to verify KubeEdge.

<br>

### <div id='1.3'> 1.3. System Configuration Diagram
The system configuration consists of a Kubernetes Cluster (Master, Worker, Edge) environment. <br>
Install the Kubernetes Cluster (Master, Worker) through Kubespray and KubeEdge in the Kubernetes Cluster and Edge environment. Pod provides middleware environments such as Database and Private registry to deploy to Container Platform portal environments to Kubernetes clusters with Container Image. <br>
The total required VM environment is **Master VM: 1, Worker VM: 1 or more, Edge VM: 1 or more**, and this document contains the installation of Master VM, Worker VM, and Edge VM to configure the Kubernetes Cluster environment.

![image 001]

<br>

### <div id='1.4'> 1.4. References
> https://docs.docker.com/engine/install/  
> https://kubeedge.io/en/docs/   
> https://github.com/kubeedge/kubeedge

<br>

## <div id='2'> 2. KubeEdge Installation

### <div id='2.1'> 2.1. Prerequisite
This installation guide is based on installation in **Ubuntu 18.04** environment. For EdgeNode, **arm64 architecture**, perform installation in **Ubuntu 20.04** environment for CRI-O installation. In this guide, the environment of EdgeNode was written based on **Ubuntu 20.04 arm64**. For KubeEdge installation, CRI-O, Kubernetes Native Cluster must be deployed on the system.

Main software and package version information required for KubeEdge installation is as follows.

|Main Software|Version|
|---|---|
|KubeEdge|v1.8.2|
|Kubernetes Native|v1.20.5|
|Kubernetes Native (Edge Node)|v1.19.3|
|CRI-O|v1.20.0|
|CRI-O (Edge Node)|v1.19.0|

The Kubernetes official guide document recommends the following when deploying clusters:

- One or more machines running a debug / rpm compatible Linux OS (Ubuntu or CentOS)
- More than 2G RAM per machine
- Two or more CPUs on a machine that is used as a control-plane node
- Full network connectivity between all systems in the cluster

#### Firewall Insformation
- Master Node

| <center>Protocol</center> | <center>Port</center> | <center>Note</center> |  
| :---: | :---: | :--- |  
| TCP | 111 | NFS PortMapper |  
| TCP | 179 | Calio BGP Network |  
| TCP | 2049 | NFS |  
| TCP | 2379-2380 | etcd server client API |  
| TCP | 6443 | kubernetes API Server |  
| TCP | 9443 | cloudcore router port |  
| TCP | 10000-10004 | cloudHub websocket port |  
| TCP | 10001 | cloudHub quic port |  
| TCP | 10002 | cloudHub https port |  
| TCP | 10003 | cloudStream streamPort |  
| TCP | 10004 | cloudStream tunnelPort |  
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

- Edge Node

| <center>Protocol</center> | <center>Port</center> | <center>Note</center> |  
| :---: | :---: | :--- |  
| TCP | 111 | NFS PortMapper |  
| TCP | 1883-1884 | eventBus mqttPort |  
| TCP | 2049 | NFS |  
| TCP | 10001 | edgeHub quic port |  
| TCP | 10250 | Kubelet API |  
| TCP | 10255 | Read-Only Kubelet API |  
| TCP | 10350 | Use kubectl logs |  
| TCP | 10550 | edgecore list-watch port |  
| TCP | 30000-32767 | NodePort Services |  
| TCP | 40001 | edgeMesh listenPort |

<br>

### <div id='2.2'> 2.2. Kubernetes Native Cluster Delployment
For KubeEdge installation, the Kubernetes cluster must be deployed in the Cloud area, and after deployment, the Edge Node must be deployed in the Edge area.

- Deploy Kubernetes Cluster through Kubespray in the Cloud area.

> https://github.com/PaaS-TA/paas-ta-container-platform/blob/master/install-guide/standalone/paas-ta-container-platform-standalone-deployment-guide-v1.2.md

<br>

### <div id='2.3'> 2.3. KubeEdge keadm Installation
Install keadm for installing KubeEdge. Super User or root authentication is required when running keadm, so proceed with the installation with **root authentication**.

- Download and install keadm on VMs that will be used as **Master Node** in Cloud and **Edge Node** in Edge.

```
$ sudo su -

# git clone https://github.com/PaaS-TA/paas-ta-container-platform-deployment.git

## When Ubuntu architecture is amd64 (ex: Cloud Area Master Node)
# cp paas-ta-container-platform-deployment/edge/keadm/amd64/keadm /usr/bin/keadm

## When Ubuntu architecture is arm64 (ex: Edge Area Edge Node)
# cp paas-ta-container-platform-deployment/edge/keadm/arm64/keadm /usr/bin/keadm
```

<br>

### <div id='2.4'> 2.4. KubeEdge CloudCore Installation
Install KubeEdge CloudCore on the Master Node in the Cloud area and proceed with the setup.

- Install CloudCore in **Master Node** at the cloud area with the keadm init command.
```
## {MASTER_PUB_IP} : Master Node Public IP
## {MASTER_PRIV_IP} : Master Node Private IP

# keadm init --advertise-address={MASTER_PUB_IP} --kubeedge-version 1.8.2 --master=https://{MASTER_PRIV_IP}:6443
```

- Get the Token value for installing Edge Core in the Edge area.
```
# keadm gettoken
```

<br>

### <div id='2.5'> 2.5. KubeEdge EdgeCore Installation
After pre-installing CRI-O on **Edge Node** in the Edge area, install KubeEdge Edge Core to proceed with the setup. In the case of EdgeNode, the installation should proceed in **Ubuntu 20.04** environment for CRI-Oarm64 installation.

- If the EdgeNode environment is **RaspberryPie**, add the following information. If it is not a raspberry pie environment, skip the following two steps and proceed from the Edge Node CRI-O installation process.
```
# vi /boot/firmware/cmdline.txt

... cgroup_enable=memory cgroup_memory=1 (Add at the very last)
```

- Before installing CRI-O, there is an APT issue in the **Raspberry Pie Ubuntu 20.04** arm64 version, and the following action is taken.
```
# killall apt apt-get

# rm /var/lib/apt/lists/lock
# rm /var/cache/apt/archives/lock
# rm /var/lib/dpkg/lock*

# dpkg --configure -a

# apt-get update
```

- Process **Rasberry Pie** Reboot.
```
# reboot
```

- Process CRI-O installation at **Edge Node**.
```
## When installing CRI-O after the Rasberry Pie Reboot, switch to Root authority.
$ sudo su -

# cd paas-ta-container-platform-deployment/edge

# source crio-install.sh
```

- Install CNI Plugin for CRI-O on**Edge Node**.
```
# mkdir -p /opt/cni/bin
# cp cni-plugins/* /opt/cni/bin/
```

- Register and launch CRI-O service at **Edge Node**.
```
# source enable-crio.sh
```

- Proceed with EdgeCore installation with the keadm join command at **Edge Node**.
```
## {MASTER_PUB_IP} : Master Node Public IP
## {GET_TOKEN} : Token value called after CloudCore installation in the Cloud area

# keadm join --cloudcore-ipport={MASTER_PUB_IP}:10000 --token={GET_TOKEN} --cgroupdriver systemd --remote-runtime-endpoint unix:///var/run/crio/crio.sock --runtimetype remote --kubeedge-version 1.8.2
```

<br>

### <div id='2.6'> 2.6. Change DaemonSet Settings
Since KubeEdge does not support Ingress and CNI at the time of writing this installation guide, measures are needed to prevent Ingress Controller and Calico CNI from being deployed on Edge Node. In addition, OpenStack will take additional measures to prevent csi cinder nodeplugin from being deployed.

- Modify DaemonSet yaml to prevent Ingress Controller from being deployed on Edge Node at **Master Node**.
```
# kubectl edit daemonsets.apps ingress-nginx-controller -n ingress-nginx
```

- Add the following content at the spec.template.spec path.
```
     affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: node-role.kubernetes.io/edge
                operator: DoesNotExist
```

- Modify DaemonSet yaml to prevent Calico CNI from being deployed on Edge Node at **Master Node**.
```
# kubectl edit daemonsets.apps calico-node -n kube-system
```

- Add the following content at the spec.template.spec path.
```
     affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: node-role.kubernetes.io/edge
                operator: DoesNotExist
```

- Modify DaemonSet yaml to prevent Node Local DNS from being deployed on Edge Node at **Master Node**.
```
# kubectl edit daemonsets.apps nodelocaldns -n kube-system
```

- Add the following content at the spec.template.spec path.
```
     affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: node-role.kubernetes.io/edge
                operator: DoesNotExist
```

- In case of OpenStack, modify DaemonSet yaml to prevent csi cinder nodeplugin on Edge Node at **Master Node**.
```
# kubectl edit daemonsets.apps csi-cinder-nodeplugin -n kube-system
```

- Add the following content at spec.template.spec path.
```
     affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: node-role.kubernetes.io/edge
                operator: DoesNotExist
```

<br>


### <div id='2.7'> 2.7. Enable the kubectl logs feature
In KubeEdge, there is a issue that cannot use kubectl logs command as basic. The installation guide provides a setting guide for activating a corresponding function.  

- Modify the cloudcore.yaml file at **Master Node**. (change enable: true)
```
# vi /etc/kubeedge/config/cloudcore.yaml
```

```
cloudStream:
  enable: true (modified)
  streamPort: 10003
  tlsStreamCAFile: /etc/kubeedge/ca/streamCA.crt
  tlsStreamCertFile: /etc/kubeedge/certs/stream.crt
  tlsStreamPrivateKeyFile: /etc/kubeedge/certs/stream.key
  tlsTunnelCAFile: /etc/kubeedge/ca/rootCA.crt
  tlsTunnelCertFile: /etc/kubeedge/certs/server.crt
  tlsTunnelPrivateKeyFile: /etc/kubeedge/certs/server.key
  tunnelPort: 10004
```

- Modify the IP information in the script to activate the kubectl logs function and execute it at the **Master Node**.
```
# cd paas-ta-container-platform-deployment/edge

# vi enable-logs.sh

export CLOUDCOREIPS="{MASTER_PUB_IP}" (Modified)
...
```

```
# source enable-logs.sh
```

- Restart cloudcore at the **Master Node**.
```
# source restart-cloudcore.sh
```

- Modify edgecore.yaml file at the **Edge Node**. (enable: true)
```
# vi /etc/kubeedge/config/edgecore.yaml
```

```
edgeStream:
  enable: true (Modified)
  handshakeTimeout: 30
  readDeadline: 15
  server: xxx.xxx.xxx.xxx:10004
  tlsTunnelCAFile: /etc/kubeedge/ca/rootCA.crt
  tlsTunnelCertFile: /etc/kubeedge/certs/server.crt
  tlsTunnelPrivateKeyFile: /etc/kubeedge/certs/server.key
  writeDeadline: 15
```

Describes how to bypass the deployment of kube-proxy on edgecore restart because edgecore restart is not possible if kube-proxy is deployed. 

- Modify edgecore.service file at the **EdgeNode**.
```
# vi /etc/kubeedge/edgecore.service
```

- Add the following at the [Service] of edgecore.service file.
```
Environment="CHECK_EDGECORE_ENVIRONMENT=false"
```

- Restart edgecore at **Edge Node**.
```
# source restart-edgecore.sh
```

<br>

### <div id='2.8'> 2.8. EdgeMesh Deployment 
Starting with KubeEdge v1.8, EdgeMesh is separated from EdgeCore modules as a separate Pod, providing the EdgeMesh Server, Agent Pod deployment guide.

- **Master Node**에서 EdgeMesh Pod 배포 전 관련 CRDs 배포를 진행한다.
```
#  kubectl apply -f edgemesh/crds/istio/
```

- **Edge Node**에서 EdgeCore설정 변경 및 서비스 재시작을 통해 EdgeNode의 List-Watch를 활성화한다.
```
# vi /etc/kubeedge/config/edgecore.yaml
```

```
modules:
  ..
  edgeMesh: (추가)
    enable: false (추가)
  ..
  metaManager:
    metaServer:
      enable: true (수정)
..
```

```
# source restart-edgecore.sh
```

- **Master Node**에서 CloudCore의 설정 변경 및 서비스 재시작을 진행한다.
```
# vi /etc/kubeedge/config/cloudcore.yaml
```

```
modules:
  ..
  dynamicController:
    enable: true (수정)
..
```

```
# source restart-cloudcore.sh
```

- **Master Node**에서 EdgeMesh Server가 배포될 VM의 호스트명 정보를 수정한다.
```
# vi edgemesh/server/06-deployment.yaml

...
spec:
  hostNetwork: true
#     use label to selector node
  nodeName: {MASTER_HOSTNAME} (수정)
...
```

- **Master Node**에서 EdgeMesh Server 배포를 진행한다.
```
# kubectl apply -f edgemesh/server/
```

- **Master Node**에서 EdgeMesh Agent 배포를 진행한다.
```
# kubectl apply -f edgemesh/agent/
```

<br>

### <div id='2.9'> 2.9. KubeEdge 설치 확인
Kubernetes Node 및 kube-system Namespace의 Pod를 확인하여 KubeEdge 설치를 확인한다.

```
# kubectl get nodes
NAME                 STATUS   ROLES                  AGE     VERSION
paasta-cp-edge       Ready    agent,edge             5m40s   v1.19.3-kubeedge-v1.8.2
paasta-cp-master     Ready    control-plane,master   39m     v1.20.5
paasta-cp-worker-1   Ready    <none>                 38m     v1.20.5
paasta-cp-worker-2   Ready    <none>                 38m     v1.20.5
paasta-cp-worker-3   Ready    <none>                 38m     v1.20.5

# kubectl get pods -n kube-system
NAME                                       READY   STATUS    RESTARTS   AGE
calico-kube-controllers-7c5b64bf96-5qdqv   1/1     Running   0          37m
calico-node-4hbw5                          1/1     Running   0          4m34s
calico-node-8q5tv                          1/1     Running   0          5m9s
calico-node-qlq5k                          1/1     Running   0          5m26s
calico-node-xc55c                          1/1     Running   0          4m53s
coredns-657959df74-grflz                   1/1     Running   0          37m
coredns-657959df74-wbdl6                   1/1     Running   0          37m
dns-autoscaler-b5c786945-cbcv9             1/1     Running   0          37m
kube-apiserver-paasta-cp-master            1/1     Running   0          36m
kube-controller-manager-paasta-cp-master   1/1     Running   1          39m
kube-proxy-2ckfd                           1/1     Running   0          38m
kube-proxy-hb8p2                           1/1     Running   0          38m
kube-proxy-nnh6d                           1/1     Running   0          38m
kube-proxy-p9srm                           1/1     Running   0          6m4s
kube-proxy-zmp95                           1/1     Running   0          38m
kube-scheduler-paasta-cp-master            1/1     Running   1          39m
metrics-server-5cd75b7749-57sc2            2/2     Running   0          37m
nginx-proxy-paasta-cp-worker-1             1/1     Running   0          38m
nginx-proxy-paasta-cp-worker-2             1/1     Running   0          38m
nginx-proxy-paasta-cp-worker-3             1/1     Running   0          38m
nodelocaldns-24vq4                         1/1     Running   0          6m4s
nodelocaldns-jjrjj                         1/1     Running   0          37m
nodelocaldns-kgzxb                         1/1     Running   0          37m
nodelocaldns-l9s47                         1/1     Running   0          37m
```

<br>

## <div id='3'> 3. KubeEdge Reset (참고)
Cloud Side, Edge Side에서 KubeEdge를 중지한다. 필수구성요소는 삭제하지 않는다.

- Cloud Side에서 cloudcore를 중지하고 kubeedge Namespace와 같은 Kubernetes Master에서 KubeEdge 관련 리소스를 삭제한다.
```
# keadm reset --kube-config=$HOME/.kube/config
```

- Edge Side에서 edgecore를 중지한다.
```
# keadm reset
```

<br>

## <div id='4'> 4. 컨테이너 플랫폼 운영자 생성 및 Token 획득 (참고)

### <div id='4.1'> 4.1. Cluster Role 운영자 생성 및 Token 획득
KubeEdge 설치 이후에 Cluster Role을 가진 운영자의 Service Account를 생성한다. 해당 Service Account의 Token은 운영자 포털에서 Super Admin 계정 생성 시 이용된다.

- Service Account를 생성한다.
```
## {SERVICE_ACCOUNT} : Service Account 명

$ kubectl create serviceaccount {SERVICE_ACCOUNT} -n kube-system
(eg. kubectl create serviceaccount k8sadmin -n kube-system)
```

- Cluster Role을 생성한 Service Account에 바인딩한다.
```
$ kubectl create clusterrolebinding {SERVICE_ACCOUNT} --clusterrole=cluster-admin --serviceaccount=kube-system:{SERVICE_ACCOUNT}
```

- 생성한 Service Account의 Token을 획득한다.
```
## {SECRET_NAME} : Mountable secrets 값 확인

$ kubectl describe serviceaccount {SERVICE_ACCOUNT} -n kube-system

$ kubectl describe secret {SECRET_NAME} -n kube-system | grep -E '^token' | cut -f2 -d':' | tr -d " "
```

### <div id='4.2'> 4.2. Namespace 사용자 Token 획득
포털에서 Namespace 생성 및 사용자 등록 이후 Token값을 획득 시 이용된다.

- Namespace 사용자의 Token을 획득한다.
```
## {SECRET_NAME} : Mountable secrets 값 확인
## {NAMESPACE} : Namespace 명

$ kubectl describe serviceaccount {SERVICE_ACCOUNT} -n {NAMESPACE}

$ kubectl describe secret {SECRET_NAME} -n {NAMESPACE} | grep -E '^token' | cut -f2 -d':' | tr -d " "
```

<br>

## <div id='5'> 5. Resource 생성 시 주의사항
사용자가 직접 Resource를 생성 시 다음과 같은 prefix를 사용하지 않도록 주의한다.

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

[image 001]:images/edge-v1.2.png

### [Index](https://github.com/PaaS-TA/Guide-eng/blob/master/README.md) > [CP Install](https://github.com/PaaS-TA/paas-ta-container-platform-guide-eng/tree/master/install-guide/Readme.md) > Edge 설치 가이드
