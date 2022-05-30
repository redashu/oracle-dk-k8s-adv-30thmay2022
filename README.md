# oracle-dk-k8s-adv-30thmay2022

## plan 

<img src="plan.png">

### any application can run in container by following below process 

<img src="process.png">

### container runtimes intro

<img src="cre_intro.png">

### Docker architecture 

<img src="darch.png">

### Docker Installation on VM 

<img src="vm.png">

### Install docker on OL (7.x)/ CENTOS / rhel cloud based VM 

```
[root@docker-host ~]# yum  install  docker  -y
Failed to set locale, defaulting to C
Loaded plugins: extras_suggestions, langpacks, priorities, update-motd
amzn2-core                                                                                                         | 3.7 kB  00:00:00     
Resolving Dependencies
--> Running transaction check
---> Package docker.x86_64 0:20.10.13-2.amzn2 will be installed
--> Processing Dependency: runc >= 1.0.0 for package: docker-20.10.13-2.amzn2.x86_64
--> Processing Dependency: libcgroup >= 0.40.rc1-5.15 for package: docker-20.10.13-2.amzn2.x86_64
--> Processing Dependency: containerd >= 1.3.2 for package: docker-20.10.13-2.amzn2.x86_64
--> Processing Dependency: pigz for package: docker-20.10.13-2.amzn2.x86_64
--> Running transaction check
---> Package containerd.x86_64 0:1.4.13-2.amzn2.0.1 will be installed
---> Package libcgroup.x86_64 0:0.41-21.amzn2 will be installed
---> Package pigz.x86_64 0:2.3.4-1.amzn2.0.1 will be installed
---> Package runc.x86_64 0:1.0.3-2.amzn2 will be installed
--> Finished Dependency Resolution
```

### activate Docker service 

```
systemctl enable --now docker 
```

### setup non root users for docker access purpose 

```
 for  i in  ashu  prateek mousumi nishtha tanvi 
> do
> useradd $i
> echo "Docker@099#"  |  passwd  $i --stdin 
> usermod -aG docker $i
> done 
Changing password for user ashu.
passwd: all authentication tokens updated successfully.
Changing password for user prateek.
passwd: all authentication tokens updated successfully.
Changing password for user mousumi.
passwd: all authentication tokens updated successfully.
Changing password for user nishtha.
passwd: all authentication tokens updated successfully.
Changing password for user tanvi.
passwd: all authentication tokens updated successfully.
```

### verify docker client with non root users 

```
ssh  ashu@34.231.236.153
ashu@34.231.236.153's password: 

       __|  __|_  )
       _|  (     /   Amazon Linux 2 AMI
      ___|\___|___|

https://aws.amazon.com/amazon-linux-2/
2 package(s) needed for security, out of 6 available
Run "sudo yum update" to apply all updates.
-bash: warning: setlocale: LC_CTYPE: cannot change locale (UTF-8): No such file or directory
[ashu@docker-host ~]$ 
[ashu@docker-host ~]$ 
[ashu@docker-host ~]$ whoami
ashu
[ashu@docker-host ~]$ docker  version 
Client:
 Version:           20.10.13
 API version:       1.41
 Go version:        go1.16.15
 Git commit:        a224086
 Built:             Thu Mar 31 19:20:32 2022
 OS/Arch:           linux/amd64
 Context:           default
 Experimental:      true

Server:
 Engine:
  Version:          20.10.13
  API version:      1.41 (minimum version 1.12)
  Go version:       go1.16.15
  Git commit:       906f57f
  Built:            Thu Mar 31 19:21:13 2022
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.4.13
  GitCommit:        9cc61520f4cd876b86e77edfeb88fbcd536d1f9d
 runc:
  Version:          1.0.3
  GitCommit:        f46b6ba2c9314cfc8caae24a32ec5fe9ef1059fe
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
[ashu@docker-host ~]$ 

```

### Note: if you are using OCI -- then VM of OL 7.x is required -- OL 8.x --default container runtime is podman 

### Docker host setup on Local VM 

[URL](https://docs.docker.com/engine/install/)


### application containerization 

<img src="appcont.png">

## application containerization 

### sample frontend app containerization 

<img src="app2.png">

### creating app to docker image 

#### step 1  - cloning app 

```
 git clone  https://github.com/microsoft/project-html-website
Cloning into 'project-html-website'...
remote: Enumerating objects: 19, done.
remote: Total 19 (delta 0), reused 0 (delta 0), pack-reused 19
Receiving objects: 100% (19/19), 462.63 KiB | 25.70 MiB/s, done.
[ashu@docker-host webapp1]$ ls
project-html-website
[ashu@docker-host webapp1]$ 


```

#### step 2 :- dockerfile write 

```
[ashu@docker-host webapp1]$ cat  Dockerfile 
FROM nginx 
#  we are pulling default nginx server image from Docker hub 
LABEL name=ashutoshh
LABEL email=ashutoshh@linux.com
# to share your contact details with image users 
COPY project-html-website  /usr/share/nginx/html/

```

### step 3. -- all the data 

```
ls  -a
.  ..  .dockerignore  Dockerfile  project-html-website
[ashu@docker-host webapp1]$ cat  Dockerfile 
FROM nginx 
#  we are pulling default nginx server image from Docker hub 
LABEL name=ashutoshh
LABEL email=ashutoshh@linux.com
# to share your contact details with image users 
COPY project-html-website  /usr/share/nginx/html/
[ashu@docker-host webapp1]$ 
[ashu@docker-host webapp1]$ 
[ashu@docker-host webapp1]$ ls  -al  project-html-website/
total 12
drwxrwxr-x 6 ashu ashu  103 May 30 05:28 .
drwxrwxr-x 3 ashu ashu   73 May 30 05:36 ..
drwxrwxr-x 8 ashu ashu  163 May 30 05:28 .git
-rw-rw-r-- 1 ashu ashu 1087 May 30 05:28 LICENSE
-rw-rw-r-- 1 ashu ashu  710 May 30 05:28 README.md
drwxrwxr-x 2 ashu ashu   22 May 30 05:28 css
drwxrwxr-x 2 ashu ashu   26 May 30 05:28 fonts
drwxrwxr-x 2 ashu ashu  147 May 30 05:28 img
-rw-rw-r-- 1 ashu ashu 2866 May 30 05:28 index.html
[ashu@docker-host webapp1]$ cat  .dockerignore 
project-html-website/.git
project-html-website/*.md
project-html-website/LICENSE


```


### Step 4 docker image build 

```
 ls -a
.  ..  .dockerignore  Dockerfile  project-html-website
[ashu@docker-host webapp1]$ docker  build  -t   ashuwebapp:v1   . 
Sending build context to Docker daemon    834kB
Step 1/4 : FROM nginx
latest: Pulling from library/nginx
42c077c10790: Pull complete 
62c70f376f6a: Pull complete 
915cc9bd79c2: Pull complete 
75a963e94de0: Pull complete 
7b1fab684d70: Pull complete 
db24d06d5af4: Pull complete 
Digest: sha256:2bcabc23b45489fb0885d69a06ba1d648aeda973fae7bb981bafbb884165e514
Status: Downloaded newer image for nginx:latest
 ---> 0e901e68141f
Step 2/4 : LABEL name=ashutoshh
 ---> Running in 3ed21c68ac78
Removing intermediate container 3ed21c68ac78
 ---> a0575e7cba94
Step 3/4 : LABEL email=ashutoshh@linux.com
 ---> Running in 8c2c848b6860
Removing intermediate container 8c2c848b6860
 ---> 6ad5bd1160ec
Step 4/4 : COPY project-html-website  /usr/share/nginx/html/
 ---> 67edff2e4744
Successfully built 67edff2e4744
Successfully tagged ashuwebapp:v1


```

### checking images 

```
docker  images
REPOSITORY    TAG       IMAGE ID       CREATED          SIZE
tanviwebapp   v1        0dffcbf4b211   23 seconds ago   142MB
ashuwebapp    v1        67edff2e4744   47 seconds ago   142MB
nginx         latest    0e901e68141f   2 days ago       142MB

```
### creating container to access sample webapp 

```
 docker  images
REPOSITORY      TAG       IMAGE ID       CREATED          SIZE
pratwebapp      v1        e28b09b0fcc0   24 minutes ago   142MB
tanviwebapp     v1        0dffcbf4b211   27 minutes ago   142MB
ashuwebapp      v1        67edff2e4744   28 minutes ago   142MB
mousumiwebapp   v1        67edff2e4744   28 minutes ago   142MB
nginx           latest    0e901e68141f   2 days ago       142MB
[ashu@docker-host webapp1]$ docker  run   -d  --name ashuc1  -p  1122:80   ashuwebapp:v1  
854758237b085847fabe6043d379c41280c2b29fad29a7c14afd32b36d8965f1
[ashu@docker-host webapp1]$ 
[ashu@docker-host webapp1]$ docker  ps
CONTAINER ID   IMAGE           COMMAND                  CREATED         STATUS         PORTS                                   NAMES
854758237b08   ashuwebapp:v1   "/docker-entrypoint.…"   5 seconds ago   Up 4 seconds   0.0.0.0:1122->80/tcp, :::1122->80/tcp   ashuc1
[ashu@docker-host webapp1]$ 


```

### clean up 

```
[ashu@docker-host webapp1]$ docker kill  ashuc1   # stop container 
ashuc1
[ashu@docker-host webapp1]$ docker rm  ashuc1 # remove container 
ashuc1
[ashu@dock

```
### image sharing between / among docker Host 

<img src="reg.png">

### pushing iamge to docker hub 

<ol>
  <li> create / sing in in Docker hub account </li>
 <li> convert image to docker hub format  </li>
 <li> login to docker hub account from docker client </li>
 <li> push image to docker hub  </li>
<li> you can logout from docker hub  </li> 
</ol>

### tagging image 

```
 docker   tag       ashuwebapp:v2           docker.io/dockerashu/ashuwebapp:v2 
```

### login to docker hub 

```
[ashu@docker-host webapp1]$ docker  login 
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: dockerashu
Password: 
WARNING! Your password will be stored unencrypted in /home/ashu/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
```

### pushing imaget to docker hub 

```
 docker  push  docker.io/dockerashu/ashuwebapp:v2 
The push refers to repository [docker.io/dockerashu/ashuwebapp]
fb1ca1535d3e: Pushed 
33e3df466e11: Mounted from library/nginx 
747b7a567071: Mounted from library/nginx 
57d3fc88cb3f: Mounted from library/nginx 
53ae81198b64: Mounted from library/nginx 
58354abe5f0e: Mounted from library/nginx 
ad6562704f37: Mounted from library/nginx 
v2: digest: sha256:300162b15fa401cfc81652a24ff5626e6668814ce150b1f0636cf7969ddeca23 size: 1780
```

### logout 

```
docker logout 
Removing login credentials for https://index.docker.io/v1/
```

