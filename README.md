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
