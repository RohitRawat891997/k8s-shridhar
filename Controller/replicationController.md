# ReplicationController
This example ReplicationController config runs three copies of the nginx web server.
```
apiVersion: v1
kind: ReplicationController
metadata:
  name: test
spec:
  replicas: 1
  selector:
    app: test
  template:
    metadata:
      name: test
      labels:
        app: test 
    spec:
      containers:
      - name: nginx
        image: nginx:1.16

```

```
controlplane:~$ kubectl  create  -f  rc.yml 
replicationcontroller/test created
controlplane:~$ kubectl  get  pod
NAME         READY   STATUS              RESTARTS   AGE
test-6rp4v   0/1     ContainerCreating   0          4s
controlplane:~$ 
controlplane:~$ 
controlplane:~$ kubectl  get  pod
NAME         READY   STATUS    RESTARTS   AGE
test-6rp4v   1/1     Running   0          11s
```
