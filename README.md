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

