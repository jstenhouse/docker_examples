FROM cassandra:3.0.2

# we need this to enable JMX monitoring
COPY conf/jmxremote.password /etc/cassandra/jmxremote.password
RUN chmod 400 /etc/cassandra/jmxremote.password

COPY conf/jmxremote.password /etc/java-8-openjdk/management/jmxremote.password
