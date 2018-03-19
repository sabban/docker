FROM openjdk:8-jre

ENV KAFKA_RELEASE_FILENAME="kafka_2.12-1.0.0"
ENV KAFKA_RELEASE_VERSION="1.0.0"
ENV TOPICS="local0.raw local1.raw local2.raw local3.raw local4.raw local5.raw local6.raw local7.raw apache.parsed auth.parsed ban_distrib.parsed bucket.ban bucket.notification cron.parsed kernel.parsed nginx.parsed rsyslog.parsed ssh.parsed suhosin.parsed unparsed"

RUN apt-get update && apt-get install -y netcat paxctl supervisor
RUN wget http://mirrors.ircam.fr/pub/apache/kafka/$KAFKA_RELEASE_VERSION/$KAFKA_RELEASE_FILENAME.tgz -O $KAFKA_RELEASE_FILENAME.tgz
RUN tar zxvf $KAFKA_RELEASE_FILENAME.tgz -C /
RUN mv /$KAFKA_RELEASE_FILENAME /kafka
RUN paxctl -cspermx $(which java)

EXPOSE 9092

RUN sed -i 's/localhost/zookeeper/g' /kafka/config/server.properties

CMD nc -l -p 10000 && cat $CI_PROJECT_DIR/hosts > /etc/hosts ; /kafka/bin/kafka-server-start.sh /kafka/config/server.properties