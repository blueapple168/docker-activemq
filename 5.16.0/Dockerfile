FROM openjdk:8-jre-alpine

ENV ACTIVEMQ_VERSION 5.16.0
ENV ACTIVEMQ apache-activemq-$ACTIVEMQ_VERSION
ENV ACTIVEMQ_TCP=61616 ACTIVEMQ_AMQP=5672 ACTIVEMQ_STOMP=61613 ACTIVEMQ_MQTT=1883 ACTIVEMQ_WS=61614 ACTIVEMQ_UI=8161
ENV SHA512_VALUE=$ACTIVEMQ-bin.tar.gz.sha512

ENV ACTIVEMQ_HOME /opt/activemq

RUN set -x && \
    mkdir -p /opt && \
    apk --update add --virtual build-dependencies curl \
    tzdata && \
    cp -r -f /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && \
    echo "Asia/Shanghai" > /etc/timezone \
    curl https://archive.apache.org/dist/activemq/$ACTIVEMQ_VERSION/$ACTIVEMQ-bin.tar.gz -o $ACTIVEMQ-bin.tar.gz && \
    curl https://archive.apache.org/dist/activemq/$ACTIVEMQ_VERSION/$ACTIVEMQ-bin.tar.gz.sha512 -o $SHA512_VALUE && \

    # Validate checksum
    if [ "$(sha512sum -c $SHA512_VALUE|awk -F':' '{print $2}')" != 'OK' ];\
       then \
         echo "sha512 values doesn't match! exiting."  && \
         exit 1; \
    fi;

RUN tar xzf $ACTIVEMQ-bin.tar.gz -C  /opt && \
    ln -s /opt/$ACTIVEMQ $ACTIVEMQ_HOME && \
    addgroup -S activemq && adduser -S -H -G activemq -h $ACTIVEMQ_HOME activemq && \
    chown -R activemq:activemq /opt/$ACTIVEMQ && \
    chown -h activemq:activemq $ACTIVEMQ_HOME && \
    apk del build-dependencies tzdata && \
    rm -rf /var/cache/apk/* && \
    rm -rf $ACTIVEMQ-bin.tar.gz \
           $ACTIVEMQ-bin.tar.gz.sha512

USER activemq

WORKDIR $ACTIVEMQ_HOME
EXPOSE $ACTIVEMQ_TCP $ACTIVEMQ_AMQP $ACTIVEMQ_STOMP $ACTIVEMQ_MQTT $ACTIVEMQ_WS $ACTIVEMQ_UI

CMD ["/bin/sh", "-c", "bin/activemq console"]
