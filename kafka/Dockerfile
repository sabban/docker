FROM openjdk:8-jre

ENV KAFKA_RELEASE_FILENAME="kafka_2.12-1.0.0"
ENV KAFKA_RELEASE_VERSION="1.0.0"

RUN apt-get update && apt-get install -y netcat paxctl golang-1.8-go git
RUN wget http://mirrors.ircam.fr/pub/apache/kafka/$KAFKA_RELEASE_VERSION/$KAFKA_RELEASE_FILENAME.tgz -O $KAFKA_RELEASE_FILENAME.tgz
RUN tar zxvf $KAFKA_RELEASE_FILENAME.tgz -C /
RUN mv /$KAFKA_RELEASE_FILENAME /kafka
RUN paxctl -cspermx $(which java)

ADD supervisor/kafka.conf /etc/supervisor/conf.d/
ADD supervisor/zookeeper.conf /etc/supervisor/conf.d/

EXPOSE 2181 9092

RUN supervisord && until echo test | nc 127.0.0.1 2181; do sleep 0.1; done  && until echo test | nc 127.0.0.1 9092; do sleep 0.1; done

RUN /kafka/bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test 
RUN supervisorctl shutdown

CMD supervisord -n

