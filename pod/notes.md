# Simple kubernetes Pod
```
apiVersion: v1
kind: Pod
metadata:
  name: test
spec:
  containers:
  - image: nginx:1.17
    name: test
```
```controlplane:~$ kubectl  create  -f  pod.yml 
pod/test created
controlplane:~$ 
controlplane:~$ kubectl  get  pod
NAME   READY   STATUS    RESTARTS   AGE
test   1/1     Running   0          2s
```
