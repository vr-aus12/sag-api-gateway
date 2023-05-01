# Installing webMethods API Gateway using Docker

## Install Oracle Virtural Box Software

(https://www.virtualbox.org/wiki/Downloads)
`
## Install the alpine linux virtual machine using virtual box

Download the alpine linux software
(https://www.alpinelinux.org/downloads/)

https://dl-cdn.alpinelinux.org/alpine/v3.17/releases/x86_64/alpine-standard-3.17.3-x86_64.iso

Create linux VM in virtual box and install alphine linux using the ISO image

![Create VM](/images/install/linux-vm-0.jpg)

![Create VM](/images/install/linux-vm.jpg)

![Create VM](/images/install/linux-vm-1.jpg)

![Create VM](/images/install/linux-vm-2.jpg)

![Create VM](/images/install/linux-vm-3.jpg)

![Create VM](/images/install/linux-vm-4.jpg)
![Create VM](/images/install/linux-vm-5.jpg)
![Create VM](/images/install/linux-vm-6.jpg)
![Create VM](/images/install/linux-vm-7.jpg)
![Create VM](/images/install/linux-vm-8.jpg)

## Install docker
Once the linux installation is completed, install docker software using the following commands

Edit the following file using the below command

```vi /etc/apk/repositories```

Uncomment all repositories by removing # in front

Then run the below commands to install docker

```
apk update
```

```
apk add docker docker-compose
```
![Create VM](/images/install/docker-2.jpg)


Run the below commands to start docker daemon
```
rc-update add docker boot
```
```
service docker start
```

Now the docker is installed

## Install SAG API Gateway
Now install SoftwareAG API Gateway and API Portal using the following commands.

Docker images are in the following links

https://hub.docker.com/u/softwareag

https://hub.docker.com/r/softwareag/devportal

https://hub.docker.com/r/softwareag/apigateway-trial


Run the below command to increase the kernel setting in linux host.
```
sysctl -w vm.max_map_count=262144
```
![Create VM](/images/install/docker-3-map-setting.jpg)


Run the below command to install seperate network in docker

```
docker network create sag-webmethods-api-network
```
![Create VM](/images/install/docker-3-create-network.jpg)


```
docker run -d -e "discovery.type=single-node" -e "xpack.security.enabled=false" -u elasticsearch --net sag-webmethods-api-network --hostname devportal-elastic --net-alias devportal-elastic docker.elastic.co/elasticsearch/elasticsearch:8.2.3
```

![Create VM](/images/install/docker-5-elastic-docker.jpg)



```
docker run -d -e SPRING_ELASTICSEARCH_REST_URIS="http://devportal-elastic:9200" --net sag-webmethods-api-network --hostname devportal-server -p 80:8083 softwareag/devportal:10.15.0.5
```

![Create VM](/images/install/docker-4-dev-portal-docker.jpg)



```
docker run -d -p 5555:5555 -p 9072:9072  --net sag-webmethods-api-network --hostname apigw-host --name apigw softwareag/apigateway-trial:10.15
```


![Create VM](/images/install/docker-6.jpg)


```
docker container ls
```

![Create VM](/images/install/docker-7.jpg)

Wait for few minutes for servers to startup. Check the below command for log status


```
docker logs apigw --follow
```

When you access the below URL for dev portal, check IP on your host

```
http://192.168.20.39/portal
```

![Create VM](/images/install/docker-8.jpg)

Use Administrator/manage username and password to log into the API portal

![Create VM](/images/install/docker-10.jpg)

When you access the below URL for Integration Server, check IP on your host

```
http://192.168.20.39:5555
```

![Create VM](/images/install/docker-9.jpg)

Use Administrator/manage username and password to log into the IS

![Create VM](/images/install/docker-11.jpg)

Under solutions -> click API Gateway, API gateway URL will open

![Create VM](/images/install/api-1.jpg)

Configure API portal details in API Gateway as below and test it.

![Create VM](/images/install/api-2.jpg)

# Create the first API and test it

Create API in the API gateway as below, import openssl file downloaded from swaggerhu Simple API template.
![Create VM](/images/install/api-3.jpg)

Activate the API, publish to API portal, enable tracing as below
![Create VM](/images/install/api-4.jpg)


![Create VM](/images/install/api-5.jpg)
In API portal, you can see the published API
![Create VM](/images/install/api-6.jpg)

Try the API from API portal, by calling get method
![Create VM](/images/install/api-7.jpg)

Now you can see the trace logs in API gateway as below
![Create VM](/images/install/api-8.jpg)

## Conclusion

We successfully installed below
Installed Oracle virtual box.
Created a Alphine linux VM
Installed docker software in the VM
Installed API gateway and portal docker containers
Created and tested first API





