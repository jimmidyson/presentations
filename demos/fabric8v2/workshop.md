# Fabric8 V2 workshop

## Pre-requisites

### Install Docker

#### Fedora
````
sudo yum install docker-io
````

* Configure Docker daemon:
````
sudo vi /etc/sysconfig/docker
````
Set:
````
OPTIONS="--selinux-enabled=true -H tcp://127.0.0.1:2375 --insecure-registry 172.0.0.0/8"
````
* Run docker:
````
sudo systemctl enable docker
sudo systemctl start docker
````
* Optional: Add useful alias to ~/.bashrc / ~/.bash_profile
````
export DOCKER_HOST=tcp://$DOCKER_IP:2375
alias kube="docker run --rm --net=host -i openshift/origin:latest cli"
````

* Create storage host dir for Docker registry images
````
sudo mkdir -p /volumes/registry/data
sudo chcon -Rt svirt_sandbox_file_t /volumes/registry/data
````

#### Mac
Follow https://docs.docker.com/installation/mac/ to install boot2docker

* Download latest release from https://github.com/boot2docker/osx-installer/releases/latest
* Run installer
* Run `Boot2Docker` in `Applications` folder
* Export some environment variables in local shell or ~/.bashrc / ~/.bash_profile:
````
export DOCKER_IP=`boot2docker ip 2> /dev/null`
export KUBERNETES_MASTER=http://$DOCKER_IP:8080
export DOCKER_HOST=tcp://$DOCKER_IP:2375
````
* Configure Docker:
````
boot2docker ssh
sudo vi /var/lib/boot2docker/profile
````
* Add:
````
DOCKER_TLS=no
EXTRA_ARGS="--insecure-registry 172.0.0.0/8"
````
* Restart docker:
````
sudo /etc/init.d/docker restart
````
* Exit boot2docker shell
* Optional: Add useful alias to ~/.bashrc / ~/.bash_profile
````
alias kube="docker run --rm --net=host -i openshift/origin:latest cli"
````

## Run fabric8 quickstart script
Fingers crossed please:
````
bash <(curl -sSL https://bit.ly/get-fabric8)
````

After a while, some service URLs should be displayed in your terminal.

Export the `DOCKER_REGISTRY` environment variable in your shell:
````
export DOCKER_REGISTRY=<Docker registry IP displayed>:5000
````
Check that's working:
````
curl http://$DOCKER_REGISTRY/v1/_ping
````
Should return some JSON (don't worry about contents)

## Deploy your first  quickstart
Git clone the quickstarts repository:
````
git clone https://github.com/fabric8io/quickstarts.git
````
Navigate to `quickstarts/war/camel-servlet`

Build the quickstart:
````
mvn clean install docker:build
````

Check the docker image has been built:
````
docker images|grep 2.2-SNAPSHOT
````

Push the docker image:
````
docker push $DOCKER_REGISTRY/quickstart/war-camel-servlet
````

Run the quickstart:
````
mvn fabric8:run
````

Go to Fabric8 console in your browser & should see under `Apps` that the application is running.

Click on services link & then on address link for `quickstart-camelservlet` service. Follow instructions to see it working - amazing! ;)

## ActiveMQ & using services
Build & deploy a broker app:
````
../../../apps/fabric8-mq
mvn clean install docker:build
docker push $DOCKER_REGISTRY/fabric8/fabric8-mq:2.2-SNAPSHOT
mvn fabric8:run
````

Back to console, see the app running.

Click on `Pods` & connect to container (little image next to status icon)

Build & deploy a producer app:

````
cd ../fabric8-mq-producer/
mvn clean install docker:build
docker push $DOCKER_REGISTRY/fabric8/fabric8-mq-producer:2.2-SNAPSHOT
mvn fabric8:run
````

See in Fabric8 console that app has started.
See in container connected tab that queue `TEST.FOO` is getting messages sent to it.

We need a consumer:
````
cd ../fabric8-mq-consumer/
mvn clean install docker:build
docker push $DOCKER_REGISTRY/fabric8/fabric8-mq-consumer:2.2-SNAPSHOT
mvn fabric8:run
````

Back to Fabric8 console...
Back to connected container tab...

## Now go forth & explore
