# Docker Project
## Description
```
پروژه داکر:
📌با استفاده از docker compose یک yml فایل بنویسید که در آن از image مربوط به elasticsearch و kibana ورژن 8.0.1 استفاده کنید. برای elasticsearch از پورت 9200 و کیبانا از پورت 5601 استفاده کنید و همچنین یک healthcheck برای بررسی وضعیت کانتینر elasticsearch بنویسید که به فاصله ی 30ثانیه سلامت کانیتنر را چک کند.
📌همچنین برای مدیریت داده ها از volume استفاده کنید و درایور آن را local قرار دهید و هر دو سرویس را در یک Network قرار دهید که درایور آن bridge و دارای subnet متفاوت باشد.( رنج subnet به دلخواه شماست)
📌برای گرفتن ایمیج از داکرهاب استفاده نمایید و در صورت بروز مشکل در نسخه ی ذکر شده میتوانید از نسخه های دیگر نیز استفاده کنید.
📌خواهشمندم که source پروژه را در repo گیت هاب قرار بدید و داکیومنت مربوط به آن را هم بنویسید.
```

## Details

### Services
```
elasticsearch:
    image: elasticsearch:8.0.1
    container_name: elasticsearch
    healthcheck:
        test: ["CMD-SHELL", "curl --silent --fail localhost:9200/_cluster/health || exit 1"]
        interval: 30s
        timeout: 30s
        retries: 3
    ports:
      - 9200:9200
    environment:
      - "ES_JAVA_OPTS=-Xmx500M -Xms500M"
      - bootstrap.memory_local=true
      - cluster.name=adlp-cluster
      - discovery.type=single-node
    volumes:
      - ./esdata:/usr/share/elasticsearch/data
    networks:
      - esnet
```
we difine our ES such that :

we have an elasticsearch service that uses elasticsearch:8.0.1 and listening on port 9200 host which mapped to 9200 container and some ENV  ,we use volumes to presist our elastic search data to prevent dataloss if anything happen to the container and have a "esnet" network as requierd.

### Kibana
```
    kibana:
    image: kibana:5.3
    container_name: kibana
    depends_on:
      elasticsearch:
          condition: service_healthy
    ports:
     - "5601:5601"
    environment:
     - ELASTICSEARCH_URL=http://elasticsearch:9200
     - XPACK_MONITORING_ENABLED=false
    networks:
      - esnet
```
we difine our EKibana such that :

we have a kibana service with kibana:5.3 and listening on port 5601 host which mapped to 5601 container ,it depends on ESservice being healthy because we want to intract with ESservice.
our service has "esnet" network too because it needs to communicate with ESservice.

### Volumes
```
    volumes:
    esdata:
        driver: local
```
we have esdata volume which presist our elasticsearch container data for us and has a local driver.

### Networks
```
    networks:
        esnet:
            driver: bridge
            ipam:
            config:
                - subnet: 172.28.0.0/16
```
we have esnet Network to have a connection between our containers. it has a bridge driver and a subnet of 172.28.0.0/16 as requierd in the description