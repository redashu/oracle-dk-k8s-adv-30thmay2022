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


### Nodeport service View 

<img src="npv.png">

### namespaces in k8s 

<img src="ns.png">

### list of existing namespaces

```
[ashu@k8s-client ~]$ kubectl get  ns
NAME                   STATUS   AGE
default                Active   26h
kube-node-lease        Active   26h
kube-public            Active   26h
kube-system            Active   26h
kubernetes-dashboard   Active   26h
[ashu@k8s-client ~]$ 
[ashu@k8s-client ~]$ 
[ashu@k8s-client ~]$ kubectl  get po -n kube-system 
NAME                                       READY   STATUS    RESTARTS       AGE
calico-kube-controllers-56cdb7c587-vkd5g   1/1     Running   1 (146m ago)   26h
calico-node-2zdfp                          1/1     Running   1 (146m ago)   26h
calico-node-hwd67                          1/1     Running   1 (146m ago)   26h
calico-node-hxvml                          1/1     Running   1 (146m ago)   26h
coredns-6d4b75cb6d-gsg5q                   1/1     Running   1 (146m ago)   26h
coredns-6d4b75cb6d-mf6fq                   1/1     Running   1 (146m ago)   26h
etcd-control-plane                         1/1     Running   1 (146m ago)   26h
kube-apiserver-control-plane               1/1     Running   1 (146m ago)   26h
kube-controller-manager-control-plane      1/1     Running   1 (146m ago)   26h

```

### creating ns

```
kubectl create  namespace  ashu-space  --dry-run=client -oyaml
apiVersion: v1
kind: Namespace
metadata:
  creationTimestamp: null
  name: ashu-space
spec: {}
status: {}
[ashu@k8s-client ~]$ kubectl create  namespace  ashu-space 
namespace/ashu-space created
[ashu@k8s-client ~]$ kubectl get  ns
NAME                   STATUS   AGE
ashu-space             Active   3s

```

### setting default namespaces 

```

[ashu@k8s-client ~]$ kubectl get  pods
No resources found in default namespace.
[ashu@k8s-client ~]$ 
[ashu@k8s-client ~]$ kubectl  config  set-context  --current --namespace=ashu-space
Context "kubernetes-admin@kubernetes" modified.
[ashu@k8s-client ~]$ 
[ashu@k8s-client ~]$ 
[ashu@k8s-client ~]$ kubectl get  pods
No resources found in ashu-space namespace.
[ashu@k8s-client ~]$ 
[ashu@k8s-client ~]$ kubectl config get-contexts 
CURRENT   NAME                          CLUSTER      AUTHINFO           NAMESPACE
*         kubernetes-admin@kubernetes   kubernetes   kubernetes-admin   ashu-space

```

### deploy pod and svc again 

```
 kubectl create -f  day3pod.yaml  -f  nodeport.yaml 
pod/ashupod1 created
service/ashulb1 created
[ashu@k8s-client yamls]$ kubectl   get  pods
NAME       READY   STATUS    RESTARTS   AGE
ashupod1   1/1     Running   0          8s
[ashu@k8s-client yamls]$ kubectl   get  svc
NAME      TYPE       CLUSTER-IP    EXTERNAL-IP   PORT(S)          AGE
ashulb1   NodePort   10.99.26.76   <none>        1234:31881/TCP   11s
[ashu@k8s-client yamls]$ 


```

### clean up 

```
 kubectl delete pod,svc --all
pod "ashupod1" deleted
service "ashulb1" deleted

```
### pod problems and Controllers in k8s 

<img src="contr.png">

### creation 

```
[ashu@k8s-client yamls]$ kubectl  get  deploy 
NAME          READY   UP-TO-DATE   AVAILABLE   AGE
ashudeploy1   1/1     1            1           8s
[ashu@k8s-client yamls]$ kubectl  get  po
NAME                           READY   STATUS    RESTARTS   AGE
ashudeploy1-78d9779666-24bt5   1/1     Running   0          37s
[ashu@k8s-client yamls]$ kubectl  delete pods ashudeploy1-78d9779666-24bt5
pod "ashudeploy1-78d9779666-24bt5" deleted
[ashu@k8s-client yamls]$ kubectl  get  po
NAME                           READY   STATUS    RESTARTS   AGE
ashudeploy1-78d9779666-5sf4x   1/1     Running   0          7s
[ashu@k8s-client yamls]$ 

```

### deploy access 

<img src="scale.png">

```
kubectl  get deplooy 
error: the server doesn't have a resource type "deplooy"
[ashu@k8s-client ~]$ kubectl  get deploy
NAME          READY   UP-TO-DATE   AVAILABLE   AGE
ashudeploy1   3/3     3            3           8m55s
[ashu@k8s-client ~]$ kubectl  get po --show-labels
NAME                           READY   STATUS    RESTARTS   AGE     LABELS
ashudeploy1-78d9779666-fjznd   1/1     Running   0          16s     app=ashudeploy1,pod-template-hash=78d9779666
ashudeploy1-78d9779666-gxgtd   1/1     Running   0          16s     app=ashudeploy1,pod-template-hash=78d9779666
ashudeploy1-78d9779666-l2nl6   1/1     Running   0          6m17s   app=ashudeploy1,pod-template-hash=78d9779666
[ashu@k8s-client ~]$ 


```

### creating service using deployment exposing 

```
kubectl  get  deploy 
NAME          READY   UP-TO-DATE   AVAILABLE   AGE
ashudeploy1   3/3     3            3           11m
[ashu@k8s-client ~]$ 

kubectl expose  deploy ashudeploy1  --type NodePort  --port 80 --name  ashulb2
service/ashulb2 exposed
[ashu@k8s-client ~]$ 
[ashu@k8s-client ~]$ kubectl delete svc ashulb1 
service "ashulb1" deleted
[ashu@k8s-client ~]$ kubectl  get  svc -owide
NAME      TYPE       CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE   SELECTOR
ashulb2   NodePort   10.108.193.204   <none>        80:32202/TCP   12s   app=ashudeploy1
[ashu@k8s-client ~]$ 

```


