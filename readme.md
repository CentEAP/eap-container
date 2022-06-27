Containers & VMs build
======================

Docker image
------------

You may build a docker image :

    docker build --tag centeap/eap-build --file docker/Dockerfile-debian .

And run it :

    docker run --interactive --tty --publish 8080:8080 --publish 9990:9990 centeap/eap-build

With a deployment, in detached mode :

    docker run --detach --publish 8080:8080 --publish 9990:9990  \
               --volume $(pwd)/myapp.war:/opt/jboss-eap/standalone/deployments/myapp.war  \
               centeap/eap-build

You may want to build it without a checkout :

    docker build --tag centeap/eap-build --file docker/Dockerfile-debian git@github.com:centeap/eap-build.git

You may choose 

* an OS (debian, centos, alpine),
* a version of JDK (default is 11), 
* a version of eap to build (default is empty AKA newest) :

    docker build --tag centeap/eap-build:7.3.9_jdk8 --build-arg JDK_VERSION=8 --build-arg EAP_VERSION=7.3.9 --file docker/Dockerfile-alpine .

Vagrant VM
----------

You may also test the build on *FreeBSD* with Vagrant :

    cd vagrant-freebsd && vagrant up && vagrant ssh
    cd eap-build
    bash -i build-eap7.sh
