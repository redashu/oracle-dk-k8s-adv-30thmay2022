# oracle-dk-k8s-adv-30thmay2022

## plan 

<img src="plan.png">

## Creating service to access pod application 

### deploy a POD -- 

```
kubectl run  ashupod1 --image=dockerashu/ashuwebapp:v2 --port 80 --dry-run=client -o yaml  >day3pod.yaml

====
kubectl  create  -f  day3pod.yaml 
pod/ashupod1 created
[ashu@k8s-client yamls]$ kubectl  get pods
NAME       READY   STATUS    RESTARTS   AGE
ashupod1   1/1     Running   0          3s

```

### to expose pod app to external world -- below service will be responsible 

<img src="svc1.png">

```
[ashu@k8s-client yamls]$ kubectl  create  service  nodeport ashulb1  --tcp  1234:80  --dry-run=client -o yaml 
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:
    app: ashulb1
  name: ashulb1
spec:
  ports:
  - name: 1234-80
    port: 1234
    protocol: TCP
    targetPort: 80
  selector:
    app: ashulb1
  type: NodePort
status:
  loadBalancer: {}
[ashu@k8s-client yamls]$ kubectl  create  service  nodeport ashulb1  --tcp  1234:80  --dry-run=client -o yaml >nodeport.yaml
[ashu@k8s-client yamls]$ 

```

### Nodeport service --

<img src="np.png">

### service will find pod using label 

<img src="label.png">

```
kubectl get po   ashupod1  --show-labels 
NAME       READY   STATUS    RESTARTS   AGE   LABELS
ashupod1   1/1     Running   0          24m   run=ashupod1
[ashu@k8s-client ~]$ 

```

### udpating selector section of pod 

```
kubectl replace -f  nodeport.yaml  --force
service "ashulb1" deleted
```

###

```
 kubectl get po   ashupod1  --show-labels 
NAME       READY   STATUS    RESTARTS   AGE   LABELS
ashupod1   1/1     Running   0          24m   run=ashupod1
[ashu@k8s-client ~]$ kubectl get svc -owide

[ashu@k8s-client ~]$ kubectl get svc -owide
NAME         TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE    SELECTOR
ashulb1      NodePort    10.108.35.230    <none>        1234:32422/TCP   115s   run=ashupod1
```
