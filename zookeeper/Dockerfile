FROM openjdk:8-jre

ENV KAFKA_RELEASE_FILENAME="kafka_2.12-1.0.0"
ENV KAFKA_RELEASE_VERSION="1.0.0"

RUN mkdir -p /kafka
RUN apt-get update && apt-get install -y netcat paxctl golang-1.8-go git
RUN wget http://mirrors.ircam.fr/pub/apache/kafka/$KAFKA_RELEASE_VERSION/$KAFKA_RELEASE_FILENAME.tgz -O /kafka/$KAFKA_RELEASE_FILENAME.tgz
RUN tar zxvf /kafka/$KAFKA_RELEASE_FILENAME.tgz -C /kafka
RUN paxctl -cspermx $(which java)

EXPOSE 2181

CMD /kafka/$KAFKA_RELEASE_FILENAME/bin/zookeeper-server-start.sh /kafka/$KAFKA_RELEASE_FILENAME/config/zookeeper.properties 2>&1 > /dev/null
