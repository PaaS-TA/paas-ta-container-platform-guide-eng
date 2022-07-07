
## Table of Contents

1. [Document Outline](#1)  
  1.1. [Purpose](#1.1)  
  1.2. [Prerequisite](#1.2)
2. [Kubernetes Resource Deployment](#2)  
  2.1. [YAML Script File for Container Deployment](#2.1)  
  2.2. [Deployment](#2.2)  
3. [Kubernetes Ingress Setting](#3)  
  3.1. [Kubespray Ingress Controller Deployment Check](#3.1)  
  3.2. [Ingress Deployment Example](#3.2)  


<br>

## <div id='1'> 1. Document Outline

### <div id='1.1'> 1.1. Purpose
This document allows you to try and learn from simple examples for basic deployment and use of Kubernetes. 

<br>

### <div id='1.2'> 1.2. Prerequisite
This usage guide was prepared based on the state in which the kubernetes cluster was created through kubespray in the Ubuntu 18.04 environment.

Access to the Master Server and perform through kubernetes CLI.


<br>

## <div id='2'> 2. Kubernetes resource Deployment

### <div id='2.1'> 2.1 YAML Script File for Container Deployment
Prepare a nginx as sample in the form of yaml which can be deployed to the Kubernetes. 
 
- Deployment Example Yaml

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80
```

- Service Example Yaml

```
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
```

### <div id='2.2'> 2.2 Deployment

>Deploy the prepared yaml.
```
ubuntu@ip-10-0-0-163:~$ vi deployment.yaml
ubuntu@ip-10-0-0-163:~$ kubectl apply -f deployment.yaml
deployment.apps/your-nginx-deployment created
service/your-nginx-svc created
```

> Check the deployed deployment, pod, and service. 
```
ubuntu@ip-10-0-0-121:~$ kubectl get deploy,pod,svc -o wide 
NAME                               READY   UP-TO-DATE   AVAILABLE   AGE    CONTAINERS   IMAGES   SELECTOR
deployment.apps/nginx-deployment   1/1     1            1           100m   nginx        nginx    app=nginx

NAME                                   READY   STATUS    RESTARTS   AGE    IP             NODE            NOMINATED NODE   READINESS GATES
pod/nginx-deployment-d46f5678b-w5jjp   1/1     Running   0          100m   10.233.113.3   ip-10-0-0-238   <none>           <none>

NAME                    TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)   AGE    SELECTOR
service/kubernetes      ClusterIP   10.233.0.1     <none>        443/TCP   143m   <none>
service/nginx-service   ClusterIP   10.233.3.131   <none>        80/TCP    13m    app=nginx
```

The nginx server is deployed as pod, and the nginx-service exposed as a service is identified..



<br>


## <div id='3'> 3. Kubernetes Ingress Setting

<br>

### <div id='3.1'> 3.1. Kubespray Ingress Controller Deployment Check


- Verify Ingress Controller deployment as Daemonset on each node in the Kubernetes cluster
Ingress is the default deployment for clusters deployed as kuberpray.

```
$ kubectl get daemonset -n ingress-nginx
NAME                       DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR            AGE
ingress-nginx-controller   3         3         3       3            3           kubernetes.io/os=linux   28m

$ kubectl get pod -n ingress-nginx
NAME                             READY   STATUS    RESTARTS   AGE
ingress-nginx-controller-255cq   1/1     Running   0          28m
ingress-nginx-controller-755nq   1/1     Running   0          28m
ingress-nginx-controller-7dwv6   1/1     Running   0          28m
```

<br>

### <div id='3.2'> 3.2. Ingress Deployment Example

- Ingress Rule setting is required after basic app and service deployment

- Ingress Example Yaml

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: nginx-ingress
  annotations:
    ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - host: "suslmk01.shop"                     ## domain to be accessed externally
    http:
      paths:
        - pathType: Prefix
          path: /
          backend:
            serviceName: nginx-service
            servicePort: 80
```

When accessing suslmk01.shop/ with a browser, pass it to 80 ports of nginx-service, which is declared as a service

- Ingress Deployment Check

```
$ kubectl get ingress
NAME            CLASS    HOSTS           ADDRESS                            PORTS   AGE
nginx-ingress   <none>   suslmk01.shop   10.10.1.13,10.10.1.15,10.10.1.19   80      2m28s
```


<br>

- Check from browser

![image](https://user-images.githubusercontent.com/67575226/111438131-3ebe0380-8747-11eb-8c88-560258df9214.png)
