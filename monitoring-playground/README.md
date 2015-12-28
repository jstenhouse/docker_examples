
## Dockerhost

```
docker-machine ip local # or name of available docker-machine 
```


## Legacy Docker Links

Using legacy docker links since I don't want to do the work for proper discovery and docker networks.


## Docker Compose

```
docker-compose up -d

docker-compose ps

docker-compose stop

docker-compose rm -f
```


## Graphite

From [jstenhouse/docker-graphite](https://github.com/jstenhouse/docker-graphite):

```
docker run --name graphite -d -p 8082:80 -p 2003:2003 -p 2004:2004 -p 7002:7002 jstenhouse/graphite:latest
```

```
http://dockerhost:8082
```


## Graphite + Statsd

```
docker run --name graphite -d -p 8082:80 -p 2003:2003 -p 8125:8125/udp -p 8126:8126 hopsoft/graphite-statsd
```

```
http://dockerhost:8082
```

**Note:** double reporting stats to carbon and statsd just to see how it all works.


## Ganglia

```
docker run --name ganglia -d -p 8083:80 -p 8649:8649 -p 8649:8649/udp mbocek/ganglia:latest
```

```
http://dockerhost:8083
```


## Grafana

```
docker run --name grafana -d -p 8084:3000 grafana/grafana:latest
```

```
http://dockerhost:8084 # admin/admin

# add new data source

# Graphite with direct url http://dockerhost:8082 (needs CORS support)
```


## Bitcoin Address Service Example

From [jstenhouse/bitcoin_address_service_example](https://github.com/jstenhouse/bitcoin_address_service_example):

```
docker run --name bitcoin_address_service -d -p 8080:8080 -p 8081:8081 --link graphite --link ganglia jstenhouse/bitcoin_address_service:latest
```

```
http://dockerhost:8080/bitcoin/addresses
```

Troubleshooting:

```
docker run --rm --name bitcoin_address_service -p 8080:8080 -p 8081:8081 --link graphite --link ganglia jstenhouse/bitcoin_address_service:latest

# or

docker run -it --rm --name bitcoin_address_service -p 8080:8080 -p 8081:8081 --link graphite --link ganglia jstenhouse/bitcoin_address_service:latest /bin/bash
```


## ELK (ElasticSearch + Logstash + Kibana)

```
docker run --name elk -d -p 9292:9292 -p 9200:9200 -p 5000:5000 -p 5000:5000/udp -e 'LOGSPOUT=ignore' -v config:/opt/logstash/conf.d pblittle/docker-logstash:latest
```

```
http://dockerhost:9292/index.html#/dashboard/file/default.json
```


## Logspout

```
docker run --name logspout -d -v /var/run/docker.sock:/tmp/docker.sock gliderlabs/logspout:latest syslog://localhost:5000
```
