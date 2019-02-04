FROM ubuntu

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && \
    apt-get install -y --no-install-recommends software-properties-common
RUN add-apt-repository -y ppa:webupd8team/java && apt-get update
RUN echo debconf shared/accepted-oracle-license-v1-1 select true | /usr/bin/debconf-set-selections
RUN apt-get install --no-install-recommends -y oracle-java8-installer && \
    rm -rf /var/cache/oracle-jdk8-installer && \
    echo "networkaddress.cache.ttl=60" >> /usr/lib/jvm/java-8-oracle/jre/lib/security/java.security && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

ENV JAVA_HOME /usr/lib/jvm/java-8-oracle
RUN groupadd -g 1000 elasticsearch && useradd elasticsearch -u 1000 -g 1000
RUN apt-get install wget apt-transport-https -y

RUN wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.5.4.deb
RUN dpkg -i elasticsearch-6.5.4.deb
 
WORKDIR /usr/share/elasticsearch

RUN set -ex && for path in data logs config config/scripts; do \
        mkdir -p "$path"; \
        chown -R elasticsearch:elasticsearch "$path"; \
    done
COPY logging.yml /usr/share/elasticsearch/config/
COPY elasticsearch.yml /etc/elasticsearch/

USER elasticsearch

ENV PATH=$PATH:/usr/share/elasticsearch/bin

CMD ["elasticsearch"]

EXPOSE 9200 9300

