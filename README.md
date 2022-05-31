# oracle-dk-k8s-adv-30thmay2022

## plan 

<img src="plan.png">

## Revision 

### docker operations using webUI 

```
docker  run -itd --name webui -p 9000:9000 -v /var/run/docker.sock:/var/run/docker.sock           --restart always   portainer/portainer 
Unable to find image 'portainer/portainer:latest' locally
latest: Pulling from portainer/portainer
772227786281: Pull complete 
96fd13befc87: Pull complete 
8b2d9b141e4d: Pull complete 
Digest: sha256:25415d1143949e5dc0b03585365dc8bbe84f443ef116dc27719dc69f23ead35e
Status: Downloaded newer image for portainer/portainer:latest
2d1ad677bcb6d6b30c69d2c4105e1159a28fb79f84b059fc00b6b27f6cf6fb04
[ashu@docker-host ~]$ docker  ps
CONTAINER ID   IMAGE                 COMMAND        CREATED         STATUS         PORTS                                       NAMES
2d1ad677bcb6   portainer/portainer   "/portainer"   4 seconds ago   Up 2 seconds   0.0.0.0:9000->9000/tcp, :::9000->9000/tcp   webui
[ashu@docker-host ~]$ 

```





