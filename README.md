# DockerAmbari

### Introduction:

This repository is forked from https://github.com/sequenceiq/docker-ambari with modified blueprint, which brings up a cluster for data scientist in Docker with minimum CPU and Memory usage.

### Request:

- Docker
- Bash-based shell
- This repository is only tested on Linux

### Install:

1. `$ source helper`, to have `amb` prefix functions

2. `$ amb-start-cluster <number-of-cluster>` startup a cluster with consul

3. Launch ambari-shell by `amb-shell`

4. `blueprint add --file data_scientist.bp` to add the blueprint for data scientist pre-installed software

5. `cluster autoassign` to deploy to your cluster

6. `cluster create --exitOnFinish true` Done!

### Nginx

In order to dynamically mapping the port from container to host, modified nginx which forked from https://hub.docker.com/r/jlordiales/nginx-consul/ is used

1. `docker build -t <nginx-image-name> .` to build it up

2. Launch the nginx container by 

```
$ docker run \
  --name nginx \
  -d \
  -p 8080:8080 -p 9995:9995 -p 9996:9996 \
  --v `pwd`/service.ctmpl:/templates/service.ctmpl \
  --dns $(docker inspect --format="{{.NetworkSettings.IPAddress}}" amb-consul) \
  --dns 172.17.0.1 \
  --dns 8.8.8.8 \
  --dns-search service.consul \
  <nginx-image-name>
  ```
  
  which forwards port `9995` for zeppelin notebook and `8080` for ambari dashboard 
  
