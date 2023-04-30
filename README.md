# Installing webMethods API Gateway using Docker

## Install Oracle Virtural Box Software

(https://www.virtualbox.org/wiki/Downloads)
`
## Install the alpine linux virtual machine using virtual box

Download the alpine linux software
(https://www.alpinelinux.org/downloads/)

https://dl-cdn.alpinelinux.org/alpine/v3.17/releases/x86_64/alpine-standard-3.17.3-x86_64.iso

Create linux VM in virtual box and install alphine linux using the ISO image

## Install docker
Once the linux installation is completed, install docker software using the following commands

```
apk update
```

```
apk add docker docker-compose
```

```
rc-update add docker boot
```
``
service docker start
``

## Install SAG API Gateway
Now install SoftwareAG API Gateway and API Portal using the following commands.

Docker images are in the following links

https://hub.docker.com/u/softwareag

https://hub.docker.com/r/softwareag/devportal

https://hub.docker.com/r/softwareag/apigateway-trial

Run the below command to increase the kernel setting in linux host.

``
sysctl -w vm.max_map_count=262144
``

Once the docker is installed, run the following docker commands to install the API portal and API Gateway
``
docker network create devportal-network
``
``
docker run -d -e "discovery.type=single-node" -e "xpack.security.enabled=false" -u elasticsearch --net devportal-network --hostname devportal-elastic --net-alias devportal-elastic docker.elastic.co/elasticsearch/elasticsearch:8.2.3
``
``
docker run -d -e SPRING_ELASTICSEARCH_REST_URIS="http://devportal-elastic:9200" --net devportal-network --hostname devportal-server -p 80:8083 softwareag/devportal:10.15.0.5
``
``
docker run -d -p 5555:5555 -p 9072:9072  --net devportal-network --hostname apigw-host --name apigw softwareag/apigateway-trial:10.15
``
