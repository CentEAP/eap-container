Containers & VMs build
======================

Docker image
------------

You may build a docker image, using eap-build directory next to the current one :

    docker build --tag centeap/eap-build --file docker/Dockerfile-ubuntu ../eap-build

And run it :

    docker run --interactive --tty --publish 8080:8080 --publish 9990:9990 centeap/eap-build

With a deployment, in detached mode :

    docker run --detach --publish 8080:8080 --publish 9990:9990  \
               --volume $(pwd)/myapp.war:/opt/jboss-eap/standalone/deployments/myapp.war  \
               centeap/eap-build

You may want to build it without a checkout :

    docker build --tag centeap/eap-build --file docker/Dockerfile-ubuntu git@github.com:centeap/eap-build.git

You may choose 

* an OS (ubuntu, Red Hat UBI, alpine),
* a version of JDK (default is 17), 
* a version of eap to build (default is empty AKA the newest one) :

    docker build --build-arg JDK_VERSION=8 --build-arg EAP_VERSION=7.3.9 \
                 --file docker/Dockerfile-alpine --tag centeap/eap-build:7.3.9_jdk8_alpine  \
                 ../eap-build

Logs are written in a build.log file in the container.
If build fails, this log file won't be readable.
Instead of this, you can add logs to the stdout with the MVN_OUTPUT argument.

    docker build --build-arg JDK_VERSION=8 --build-arg EAP_VERSION=7.3.9 \
                 --build-arg MVN_OUTPUT=3 \
                 --file docker/Dockerfile-alpine --tag centeap/eap-build:7.3.9_jdk8_alpine  \
                 ../eap-build


Vagrant VM
----------

You may also test the build on *FreeBSD* with Vagrant :

    cd vagrant-freebsd && vagrant up && vagrant ssh
    cd eap-build
    bash -i build-eap7.sh
