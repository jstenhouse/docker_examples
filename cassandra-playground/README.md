## Cassandra Playground

Play with a cassandra cluster in Docker.

### No Docker Compose

Due to the bootstraping problem discussed below, docker compose doesn't completely work. Also, only start one node at a time -- there can be errors from two nodes starting at the same time.

### Bootstraping

There needs to be a seed node setup first and I couldn't figure out how to do it appropriately with a compose file. Bootstraping funtimes.

```
docker run --name cassandra_seed --rm -e LOCAL_JMX=no jstenhouse/cassandra:3.0.2
```

After the seed is running you can then boot up the other instances. Check with `docker logs -f cassandra_seed`.

```
export CASSANDRA_SEED_HOST=$(docker inspect --format='{{ .NetworkSettings.IPAddress }}' cassandra_seed) && echo $CASSANDRA_SEED_HOST

docker run --name cassandra_0 --rm -e CASSANDRA_SEEDS=$CASSANDRA_SEED_HOST -e LOCAL_JMX=no jstenhouse/cassandra:3.0.2

docker run --name cassandra_1 --rm -e CASSANDRA_SEEDS=$CASSANDRA_SEED_HOST -e LOCAL_JMX=no jstenhouse/cassandra:3.0.2
```

### Build Image

**Note:** I needed to build a new cassandra docker image to open JMX monitoring port

```
docker build -t jstenhouse/cassandra:3.0.2 .
```

### Troubleshooting

```
export CASSANDRA_SEED_HOST=$(docker inspect --format='{{ .NetworkSettings.IPAddress }}' cassandra_seed) && echo $CASSANDRA_SEED_HOST

docker run -it --rm -e CASSANDRA_SEED_HOST=$CASSANDRA_SEED_HOST jstenhouse/cassandra:3.0.2 sh -c 'exec nodetool -h $CASSANDRA_SEED_HOST -u controlRole -pw password status'
```
