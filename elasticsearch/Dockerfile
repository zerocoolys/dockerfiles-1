FROM java:8-jre 

MAINTAINER Jason

# grab gosu for easy step-down from root
#RUN gpg --keyserver ha.pool.sks-keyservers.net --recv-keys B42F6819007F00F88E364FD4036A9C25BF357DD4
#RUN arch="$(dpkg --print-architecture)" \
#         && set -x \
#         && curl -o /usr/local/bin/gosu -fSL "https://github.com/tianon/gosu/releases/download/1.3/gosu-$arch" \
#         && curl -o /usr/local/bin/gosu.asc -fSL "https://github.com/tianon/gosu/releases/download/1.3/gosu-$arch.asc" \
#         && gpg --verify /usr/local/bin/gosu.asc \
#         && rm /usr/local/bin/gosu.asc \
#         && chmod +x /usr/local/bin/gosu

ENV ES_VERSION 2.0.0

RUN wget -qO /tmp/es.tgz https://download.elasticsearch.org/elasticsearch/elasticsearch/elasticsearch-$ES_VERSION.tar.gz && \
  cd /usr/share && \
  tar xf /tmp/es.tgz && \
  rm /tmp/es.tgz

ENV ES_HOME /usr/share/elasticsearch-$ES_VERSION
RUN useradd -d $ES_HOME -M -r elasticsearch && \
  chown -R elasticsearch: $ES_HOME

RUN echo "vm.max_map_count = 2126114" >> /etc/sysctl.conf
RUN echo "vm.overcommit_memory = 1" >> /etc/sysctl.conf

RUN echo "elasticsearch soft memlock unlimited" >> /etc/security/limits.conf
RUN echo "elasticsearch hard memlock unlimited" >> /etc/security/limits.conf

RUN $ES_HOME/bin/plugin install lmenezes/elasticsearch-kopf/2.0
RUN $ES_HOME/bin/plugin install mobz/elasticsearch-head
RUN $ES_HOME/bin/plugin install license
RUN $ES_HOME/bin/plugin install marvel-agent
RUN $ES_HOME/bin/plugin install mapper-murmur3

RUN mkdir /data /conf & chown elasticsearch: /data /conf
VOLUME ["/data","/conf"]

ADD start /start

WORKDIR $ES_HOME

#EXPOSE 9200 9300
#USER elasticsearch
CMD ["/start"]
