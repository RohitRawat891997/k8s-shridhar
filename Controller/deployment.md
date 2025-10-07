# Deploymnet Controller
The following is an example of a Deployment. It creates a ReplicaSet to bring up one mysql Pods:
```
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: demo
  name: demo
spec:
  replicas: 1
  selector:
    matchLabels:
      app: demo
  template:
    metadata:
      labels:
        app: demo
    spec:
      containers:
      - image: mysql:5.6
        name: mysql
        env:
          - name: MYSQL_ROOT_PASSWORD
            value: redhat

controlplane:~$ 
controlplane:~$ kubectl  create  deploy  demo  --image=mysql:5.6  --dry-run -o yaml  >  deploy.yml
W1007 18:33:46.342545   32039 helpers.go:703] --dry-run is deprecated and can be replaced with --dry-run=client.
controlplane:~$ 
controlplane:~$ vim  deploy.yml 
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: demo
  name: demo
spec:
  replicas: 1
  selector:
    matchLabels:
      app: demo
  template:
    metadata:
      labels:
        app: demo
    spec:
      containers:
      - image: mysql:5.6
        name: mysql
        env:
          - name: MYSQL_ROOT_PASSWORD
            value: redhat

  :wq!
  
controlplane:~$ 
controlplane:~$ kubectl  create  -f   deploy.yml 
deployment.apps/demo created
controlplane:~$ 
controlplane:~$ kubectl  get  deploy
NAME   READY   UP-TO-DATE   AVAILABLE   AGE
demo   1/1     1            1           21s
controlplane:~$ 
controlplane:~$ kubectl  get  po    
NAME                    READY   STATUS    RESTARTS   AGE
demo-767c8d474d-fmvjp   1/1     Running   0          32s
controlplane:~$ 
controlplane:~$
controlplane:~$ 
controlplane:~$ kubectl  exec  -it  demo-767c8d474d-fmvjp -- bash
root@demo-767c8d474d-fmvjp:/# mysql -u root -predhat
Warning: Using a password on the command line interface can be insecure.
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 1
Server version: 5.6.51 MySQL Community Server (GPL)

Copyright (c) 2000, 2021, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> create database shridhar;
Query OK, 1 row affected (0.00 sec)

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| shridhar           |
+--------------------+
4 rows in set (0.00 sec)

mysql> 
mysql> 
mysql> exit
Bye
root@demo-767c8d474d-fmvjp:/# 
root@demo-767c8d474d-fmvjp:/# exit
exit
controlplane:~$ 
controlplane:~$ kubectl  get  po
NAME                    READY   STATUS    RESTARTS   AGE
demo-767c8d474d-fmvjp   1/1     Running   0          6m49s

controlplane:~$ 
controlplane:~$ kubectl  describe  deploy  demo     
Name:                   demo
Namespace:              default
CreationTimestamp:      Tue, 07 Oct 2025 18:35:28 +0000
Labels:                 app=demo
Annotations:            deployment.kubernetes.io/revision: 1
Selector:               app=demo
Replicas:               1 desired | 1 updated | 1 total | 1 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=demo
  Containers:
   mysql:
    Image:      mysql:5.6
    Port:       <none>
    Host Port:  <none>
    Environment:
      MYSQL_ROOT_PASSWORD:  redhat
    Mounts:                 <none>
  Volumes:                  <none>
  Node-Selectors:           <none>
  Tolerations:              <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      True    MinimumReplicasAvailable
  Progressing    True    NewReplicaSetAvailable
OldReplicaSets:  <none>
NewReplicaSet:   demo-767c8d474d (1/1 replicas created)
Events:
  Type    Reason             Age    From                   Message
  ----    ------             ----   ----                   -------
  Normal  ScalingReplicaSet  7m27s  deployment-controller  Scaled up replica set demo-767c8d474d from 0 to 1
controlplane:~$ 

controlplane:~$ 
controlplane:~$ kubectl   scale --replicas=10  deploy demo 
deployment.apps/demo scaled
```
