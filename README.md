# docker-activemq
===============

[![Build Status](https://travis-ci.org/blueapple/docker-activemq.svg?branch=master)](https://travis-ci.org/blueapple/docker-activemq)
[![Docker Pulls](https://img.shields.io/docker/pulls/blueapple/activemq.svg?maxAge=2592000)](https://hub.docker.com/r/blueapple/activemq/)

[Docker](https://www.docker.io/) file for trusted builds of [ActiveMQ](http://activemq.apache.org/) on https://registry.hub.docker.com/u/rmohr/activemq/.

Run the latest container with:

    docker pull blueapple/activemq
    docker run -p 61616:61616 -p 8161:8161 blueapple/activemq

The JMX broker listens on port 61616 and the Web Console on port 8161.

Image Tags
----------

    blueapple/activemq:latest (blueapple/activemq:5.16.0)
    blueapple/activemq:5.16.0-alpine

Port Map
--------

    61616 JMS
    8161  UI
    5672  AMQP  (since `blueapple/activemq:5.12.1`)
    61613 STOMP (since `blueapple/activemq:5.12.1`)
    1883  MQTT  (since `blueapple/activemq:5.12.1`)
    61614 WS    (since `blueapple/activemq:5.12.1`)

Customizing configuration and persistence location
--------------------------------------------------
By default data and configuration is stored inside the container and will be
lost after the container has been shut down and removed. To persist these
files you can mount these directories to directories on your host system:

    docker run -p 61616:61616 -p 8161:8161 \
               -v /your/persistent/dir/conf:/opt/activemq/conf \
               -v /your/persistent/dir/data:/opt/activemq/data \
               blueapple/activemq

ActiveMQ expects that some configuration files already exists, so they won't be
created automatically, instead you have to create them on your own before
starting the container. If you want to start with the default configuration you
can initialize your directories using some intermediate container:

    docker run --user root --rm -ti \
      -v /your/persistent/dir/conf:/mnt/conf \
      -v /your/persistent/dir/data:/mnt/data \
      blueapple/activemq:5.15.4-alpine /bin/sh

This will bring up a shell, so you can just execute the following commands
inside this intermediate container to copy the default configuration to your
host directory:

    chown activemq:activemq /mnt/conf
    chown activemq:activemq /mnt/data
    cp -a /opt/activemq/conf/* /mnt/conf/
    cp -a /opt/activemq/data/* /mnt/data/
    exit

The last command will stop and remove the intermediate container. Your
directories are now initialized and you can run ActiveMQ as described above.

