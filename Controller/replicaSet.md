# ReplicaSet Controller
```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: frontend
  labels:
    app: guestbook
    tier: frontend
spec:
  # modify replicas according to your case
  replicas: 3
  selector:
    matchLabels:
      tier: frontend
  template:
    metadata:
      labels:
        tier: frontend
    spec:
      containers:
      - name: nginx 
        image: nginx:1.17
```
you can check for the Pods brought up:

```
controlplane:~$ kubectl  create  -f  rs.yml 
replicaset.apps/frontend created
controlplane:~$ 
controlplane:~$ kubectl  get  pod  -owide
NAME             READY   STATUS    RESTARTS   AGE     IP             NODE           NOMINATED NODE   READINESS GATES
frontend-lz7tk   1/1     Running   0          3m13s   192.168.0.4    controlplane   <none>           <none>
frontend-tcf6w   1/1     Running   0          3m13s   192.168.1.11   node01         <none>           <none>
frontend-trbl6   1/1     Running   0          3m13s   192.168.1.10   node01         <none>           <none>
controlplane:~$ 
controlplane:~$ 
```
