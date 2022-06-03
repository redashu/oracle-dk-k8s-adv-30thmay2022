# oracle-dk-k8s-adv-30thmay2022

## plan 

<img src="plan.png">

### Installing docker on Ubuntu 20.04. vm 

## steps 

### step 1 

```
sudo apt-get update 
Hit:1 http://us-east-1.ec2.archive.ubuntu.com/ubuntu focal InRelease
Get:2 http://us-east-1.ec2.archive.ubuntu.com/ubuntu focal-updates InRelease [114 kB]
Get:3 http://us-east-1.ec2.archive.ubuntu.com/ubuntu focal-backports InRelease [108 kB]
Get:4 http://security.ubuntu.com/ubuntu focal-security InRelease [114 kB]
Get:5 http://us-east-1.ec2.archive.ubuntu.com/ubuntu focal/universe amd64 Packages [8628 kB]
Get:6 http://us-east-1.ec2.archive.ubuntu.com/ubuntu focal/universe Translation-en [5124 kB]
Get:7 http://us-east-1.ec2.archive.ubuntu.
```

### Install docker 

[use_this_link](https://docs.docker.com/engine/install/ubuntu/)

```
curl -fsSL https://get.docker.com -o get-docker.sh
ubuntu@ip-172-31-17-25:~$ ls
get-docker.sh
ubuntu@ip-172-31-17-25:~$ sudo bash get-docker.sh 
# Executing docker install script, commit: 3255aa3919e7281693f62855b9d543bb50f04957
+ sh -c 'apt-get update -qq >/dev/null'
+ sh -c 'DEBIAN_FRONTEND=noninteractive apt-get install -y -qq apt-transport-https ca-certificates curl >/dev/null'
+ sh -c 'mkdir -p /etc/apt/keyrings && chmod -R 0755 /etc/apt/keyrings'
+ sh -c 'curl -fsSL "https://download.docker.com/linux/ubuntu/gpg" | gpg --dearmor --yes -o /etc/apt/keyrings/docker.gpg'
+ sh -c 'chmod a+r /etc/apt/keyrings/docker.gpg'
+ sh -c 'echo "deb [arch=amd64 signed-by=/etc/apt/keyrin

```

### adding non root user to docker group 

```
root@ip-172-31-17-25:~# grep -i docker  /etc/group
docker:x:998:
root@ip-172-31-17-25:~# usermod -aG docker  ubuntu 
root@ip-172-31-17-25:~# 

```

### setup k8s single node cluster on OBUNTU / OEL vm -- where docker is already running 

[use_link](https://minikube.sigs.k8s.io/docs/start/)

### Installing minikube on Ubuntu 

```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 69.2M  100 69.2M    0     0  92.8M      0 --:--:-- --:--:-- --:--:-- 92.7M
ubuntu@ip-172-31-17-25:~$ ls
get-docker.sh  minikube-linux-amd64
ubuntu@ip-172-31-17-25:~$ sudo install minikube-linux-amd64 /usr/local/bin/minikube
ubuntu@ip-172-31-17-25:~$ 
ubuntu@ip-172-31-17-25:~$ minikube version 
minikube version: v1.25.2
commit: 362d5fdc0a3dbee389b3d3f1034e8023e72bd3a7
ubuntu@ip-172-31-17-25:~$ 

```

### using minikube to Install k8s cluster 

```
ubuntu@ip-172-31-17-25:~$ minikube start  --driver=docker 
😄  minikube v1.25.2 on Ubuntu 20.04 (xen/amd64)
✨  Using the docker driver based on user configuration
👍  Starting control plane node minikube in cluster minikube
🚜  Pulling base image ...
💾  Downloading Kubernetes v1.23.3 preload ...
    > preloaded-images-k8s-v17-v1...: 505.68 MiB / 505.68 MiB  100.00% 67.32 Mi
    > gcr.io/k8s-minikube/kicbase: 379.06 MiB / 379.06 MiB  100.00% 38.98 MiB p
🔥  Creating docker container (CPUs=2, Memory=2200MB) ...
🐳  Preparing Kubernetes v1.23.3 on Docker 20.10.12 ...
    ▪ kubelet.housekeeping-interval=5m
    ▪ Generating certificates and keys ...
    ▪ Booting up control plane ...
    ▪ Configuring RBAC rules ...
🔎  Verifying Kubernetes components...
    ▪ Using image gcr.io/k8s-minikube/storage-provisioner:v5
🌟  Enabled addons: storage-provisioner, default-storageclass
💡  kubectl not found. If you need it, try: 'minikube kubectl -- get pods -A'
🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default
```

### install kubectl to access

```
 curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   154  100   154    0     0   3850      0 --:--:-- --:--:-- --:--:--  3948
100 43.5M  100 43.5M    0     0  71.7M      0 --:--:-- --:--:-- --:--:--  100M
ubuntu@ip-172-31-17-25:~$ ls
get-docker.sh  kubectl  minikube-linux-amd64
ubuntu@ip-172-31-17-25:~$ sudo mv kubectl  /usr/bin/
ubuntu@ip-172-31-17-25:~$ 
ubuntu@ip-172-31-17-25:~$ sudo chmod +x  /usr/bin/kubectl 
ubuntu@ip-172-31-17-25:~$ 
ubuntu@ip-172-31-17-25:~$ kubectl version --client -oyaml
clientVersion:
  buildDate: "2022-05-24T12:26:19Z"
  compiler: gc
  gitCommit: 3ddd0f45aa91e2f30c70734b175631bec5b5825a
  gitTreeState: clean
  gitVersion: v1.24.1
  goVersion: go1.18.2
  major: "1"
  minor: "24"
  platform: linux/amd64
kustomizeVersion: v4.5.4

ubuntu@ip-172-31-17-25:~$ kubectl  get  nodes
NAME       STATUS   ROLES                  AGE    VERSION
minikube   Ready    control-plane,master   117s   v1.23.3
ubuntu@ip-172-31-17-25:~$ kubectl  get  ns
NAME              STATUS   AGE
default           Active   2m3s
kube-node-lease   Active   2m4s
kube-public       Active   2m5s
kube-system       Active   2m5s
ubuntu@ip-172-31-17-25:~$ kubectl get po 
No resources found in default namespace.

```

### more mInikube commands 

```
 31  kubectl  get  nodes
   32  kubectl  get  ns
   33  kubectl get po 
   34  kubectl run pod1 --image=nginx --port 80 
   35  kubectl  get po 
   36  kubectl  get po  -owide
   37  kubectl  expose pod pod1  --type NodePort --port 80 --name lb1
   38  kubectl get svc
   39  minikube ip 
   40  hostname -i 
   41  kubectl get svc
   42  curl  http://172.31.17.25:30385
   43  curl  http://192.168.49.2:30385
   44  minikube ip 
   45  history 
ubuntu@ip-172-31-17-25:~$ 
ubuntu@ip-172-31-17-25:~$ 
ubuntu@ip-172-31-17-25:~$ 
ubuntu@ip-172-31-17-25:~$ 
ubuntu@ip-172-31-17-25:~$ 
ubuntu@ip-172-31-17-25:~$ minikube status 
minikube
type: Control Plane
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured

ubuntu@ip-172-31-17-25:~$ minikube stop 
✋  Stopping node "minikube"  ...
🛑  Powering off "minikube" via SSH ...
🛑  1 node stopped.
ubuntu@ip-172-31-17-25:~$ kubectl  get no
The connection to the server localhost:8080 was refused - did you specify the right host or port?
ubuntu@ip-172-31-17-25:~$ 
ubuntu@ip-172-31-17-25:~$ 
ubuntu@ip-172-31-17-25:~$ minikube start
😄  minikube v1.25.2 on Ubuntu 20.04 (xen/amd64)
✨  Using the docker driver based on existing profile
👍  Starting control plane node minikube in cluster minikube
🚜  Pulling base image ...
🔄  Restarting existing docker container for "minikube" ...
🐳  Preparing Kubernetes v1.23.3 on Docker 20.10.12 ...
    ▪ kubelet.housekeeping-interval=5m
🔎  Verifying Kubernetes components...
    ▪ Using image gcr.io/k8s-minikube/storage-provisioner:v5
🌟  Enabled addons: storage-provisioner, default-storageclass
🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default
ubuntu@ip-172-31-17-25:~$ kubectl  get no
NAME       STATUS   ROLES                  AGE     VERSION
minikube   Ready    control-plane,master   5m43s   v1.23.3
ubuntu@ip-172-31-17-25:~$ kubectl  get po
NAME   READY   STATUS    RESTARTS      AGE
pod1   1/1     Running   1 (53s ago)   2m57s
```

## RBAC and Namespace restrictions 

### creating Namespace --

```
kubectl create  ns  ashu-customer
namespace/ashu-customer created

```

### every namespace has default service account 

```
kubectl get  sa  -n  ashu-customer
NAME      SECRETS   AGE
default   0         111s
```

### creating service account 

```
kubectl create  sa  dev      -n  ashu-customer
serviceaccount/dev created
[ashu@k8s-client ~]$ kubectl create  sa  test      -n  ashu-customer
serviceaccount/test created
[ashu@k8s-client ~]$ 
[ashu@k8s-client ~]$ 
[ashu@k8s-client ~]$ kubectl get  sa  -n  ashu-customer
NAME      SECRETS   AGE
default   0         4m18s
dev       0         12s
test      0         5s

```

### creating token for dev service account using secret 

```
[ashu@k8s-client ~]$ cat  dev-secret.yaml 
apiVersion: v1
kind: Secret
metadata:
  name: dev-token
  annotations:
    kubernetes.io/service-account.name: dev
type: kubernetes.io/service-account-token
[ashu@k8s-client ~]$ kubectl apply -f  dev-secret.yaml -n ashu-customer  
secret/dev-token created
[ashu@k8s-client ~]$ 
[ashu@k8s-client ~]$ kubectl  get  sa -n ashu-customer 
NAME      SECRETS   AGE
default   0         21m
dev       0         17m
test      0         17m
[ashu@k8s-client ~]$ kubectl  get  secret  -n ashu-customer 
NAME        TYPE                                  DATA   AGE
dev-token   kubernetes.io/service-account-token   3      17s
[ashu@k8s-client ~]$ 

```

### getting token 

```
kubectl  describe   secret  dev-token   -n ashu-customer 
Name:         dev-token
Namespace:    ashu-customer
Labels:       <none>
Annotations:  kubernetes.io/service-account.name: dev
              kubernetes.io/service-account.uid: 65e27864-7370-4380-9b79-4d4c0e8a9a42

Type:  kubernetes.io/service-account-token

Data
====
ca.crt:     1099 bytes
namespace:  13 bytes
token:      eyJhbGciOiJSUzI1NiIsImtpZCI6InNuQ1dUMUdIanJvaWlDWTNONzVxNkV3QkFkMDljUmhLUnQxdHg2ZVh3QnMifQ.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9
```

### image you being developer you got the kubernetes config file 

```
ashu@k8s-client yamls]$ mkdir  rbac_testing 
[ashu@k8s-client yamls]$ 
[ashu@k8s-client yamls]$ cp  dev-conf.yaml   rbac_testing/
[ashu@k8s-client yamls]$ 
[ashu@k8s-client yamls]$ cd  rbac_testing/
[ashu@k8s-client rbac_testing]$ ls
dev-conf.yaml
[ashu@k8s-client rbac_testing]$ mv  dev-conf.yaml   dev-config 
[ashu@k8s-client rbac_testing]$ ls
dev-config
```

### testing 

```
 kubectl   get  nodes   --kubeconfig  dev-config 
Error from server (Forbidden): nodes is forbidden: User "system:serviceaccount:ashu-customer:dev" cannot list resource "nodes" in API group "" at the cluster scope
[ashu@k8s-client rbac_testing]$ kubectl   get  pods   --kubeconfig  dev-config 
Error from server (Forbidden): pods is forbidden: User "system:serviceaccount:ashu-customer:dev" cannot list resource "pods" in API group "" in the namespace "ashu-customer"
```
### creating roles for ashu-customer namespace 

```
 kubectl  create   role  pod-role  --resource=pod --verb=list,get,create    -n ashu-customer 
role.rbac.authorization.k8s.io/pod-role created
[ashu@k8s-client ~]$ 
[ashu@k8s-client ~]$ kubectl  get  roles  -n ashu-customer 
NAME       CREATED AT
pod-role   2022-06-03T06:19:16Z
[ashu@k8s-client ~]$ kubectl  create   role  deploy-role  --resource=deployment --verb=list,get,create,delete    -n ashu-customer 
role.rbac.authorization.k8s.io/deploy-role created
[ashu@k8s-client ~]$ kubectl  get  roles  -n ashu-customer 
NAME          CREATED AT
deploy-role   2022-06-03T06:19:59Z
pod-role      2022-06-03T06:19:16Z
[ashu@k8s-client ~]$ 

```

### create rolebinding 

```
kubectl  get  sa   -n ashu-customer 
NAME      SECRETS   AGE
default   0         82m
dev       0         77m
test      0         77m
[ashu@k8s-client ~]$ kubectl  get  roles  -n ashu-customer 
NAME          CREATED AT
deploy-role   2022-06-03T06:19:59Z
pod-role      2022-06-03T06:19:16Z
[ashu@k8s-client ~]$ 
[ashu@k8s-client ~]$ 
[ashu@k8s-client ~]$ kubectl create  rolebinding bind1 --role=pod-role --serviceaccount=ashu-custerm:dev  -n ashu-customer
rolebinding.rbac.authorization.k8s.io/bind1 created
[ashu@k8s-client ~]$ 
[ashu@k8s-client ~]$ kubectl  get  rolebinding   -n ashu-customer 
NAME    ROLE            AGE
bind1   Role/pod-role   17s

```





