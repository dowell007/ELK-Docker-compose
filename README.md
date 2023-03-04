# ELK-Docker-compose
version: '3'
services:
  elasticsearch:
    image: docker.elastic.co/elasticsearch/elasticsearch:7.12.0
    container_name: elasticsearch
    environment:
      - discovery.type=single-node
      - ES_JAVA_OPTS=-Xms512m -Xmx512m
    ports:
      - "9200:9200"
      - "9300:9300"
    volumes:
      - esdata:/usr/share/elasticsearch/data
  logstash:
    image: docker.elastic.co/logstash/logstash:7.12.0
    container_name: logstash
    environment:
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
    volumes:
      - ./logstash/pipeline:/usr/share/logstash/pipeline
    ports:
      - "5044:5044"
  kibana:
    image: docker.elastic.co/kibana/kibana:7.12.0
    container_name: kibana
    ports:
      - "5601:5601"
    environment:
      ELASTICSEARCH_URL: http://elasticsearch:9200
    depends_on:
      - elasticsearch
volumes:
  esdata:
    driver: local
