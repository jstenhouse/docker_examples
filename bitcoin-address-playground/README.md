
## Legacy Docker Links

Using legacy docker links since I don't want to do the work for proper discovery.


## Graphite

```
docker run --name graphite -d -p 8082:80 -p 2003:2003 -p 2004:2004 -p 7002:7002 creativearea/graphite:latest
```


## Bitcoin Address Service Example

From [bitcoin_address_service_example](https://github.com/jstenhouse/bitcoin_address_service_example):

```
docker run --name bitcoin_address_service -d -p 8080:8080 -p 8081:8081 --link graphite bitcoin_address_service:latest
```

Troubleshooting:

```
docker run -it --rm --name bitcoin_address_service -p 8080:8080 -p 8081:8081 --link graphite bitcoin_address_service:latest /bin/bash
```


## Cassandra

TODO
