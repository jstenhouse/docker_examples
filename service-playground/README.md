## Service Playground

Uses [jstenhouse/discovery_example](https://github.com/jstenhouse/discovery_example) and [etcd](https://github.com/coreos/etcd) to register two service locations: `/ping` and `/pong`. The third service `/services` uses etcd to discover the others.

### Docker Compose

```
docker-compose up -d

docker-compose ps

docker-compose stop

docker-compose rm -f
```

### Dockerhost

```
docker-machine ip local # or name of available docker-machine 
```

```
http://dockerhost:8080/pong
```

```
http://dockerhost:8081/pong
```

```
http://dockerhost:8082/services
```

### TODO

- Hystrix
