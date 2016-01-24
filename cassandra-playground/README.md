## Cassandra Playground

Play with a cassandra cluster in Docker.

### No Docker Compose

Due to the bootstraping problem discussed below, docker compose doesn't completely work.

### Bootstraping

There needs to be a seed node setup first and I couldn't figure out how to do it appropriately with a compose file. Bootstraping funtimes.

```
docker run -d --name cassandra_seed -e LOCAL_JMX=no jstenhouse/cassandra:3.0.2
```

After the seed is running you can then boot up the other instances. Check with `docker logs -f cassandra_seed`.

```
docker run -d --name cassandra_0 -e CASSANDRA_SEEDS="$(docker inspect --format='{{ .NetworkSettings.IPAddress }}' cassandra_seed)" -e LOCAL_JMX=no jstenhouse/cassandra:3.0.2

docker run -d --name cassandra_1 -e CASSANDRA_SEEDS="$(docker inspect --format='{{ .NetworkSettings.IPAddress }}' cassandra_seed)" -e LOCAL_JMX=no jstenhouse/cassandra:3.0.2
```

### Shutdown

```
docker stop cassandra_seed cassandra_0 cassandra_1 && docker rm cassandra_seed cassandra_0 cassandra_1
```

### Build Image

**Note:** I needed to build a new cassandra docker image to open JMX monitoring port

```
docker build -t jstenhouse/cassandra:3.0.2 .
```

### Troubleshooting

```
docker inspect --format='{{ .NetworkSettings.IPAddress }}' cassandra_seed

docker run -it --rm jstenhouse/cassandra:3.0.2 sh -c 'exec nodetool -h <cassandra-seed-ip> -u controlRole -pw password status'
```
