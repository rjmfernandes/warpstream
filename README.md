# WarpStream

- [WarpStream](#warpstream)
  - [Install](#install)
  - [Hello World](#hello-world)
- [Minio](#minio)


## Install

For installing:

```shell
curl https://console.warpstream.com/install.sh | bash
```

Change to your user profile path:

```shell
source /Users/ruifernandes/.zshrc
warpstream demo
```

Access developer console (something like): https://console.warpstream.com/virtual_clusters/vci_7f8dea20_17b8_423e_8b14_5109fd55667f/overview

It is using local data directory on something like: `/var/folders/h6/9mpx8_fx5_s9fmzn4yzwkktm0000gp/T/warpstream_demo539057668`

It will execute something like:

```shell
WARPSTREAM_AVAILABILITY_ZONE=DEMO WARPSTREAM_LOG_LEVEL=warn warpstream agent \
				--defaultVirtualClusterID=vci_7f8dea20_17b8_423e_8b14_5109fd55667f \
				--agentKey=aks_7b36720e60669ca9da0ddffe96e5bb483de26c8353260c1cb7c3fc8da2257b07 \
				--bucketURL=file:///var/folders/h6/9mpx8_fx5_s9fmzn4yzwkktm0000gp/T/warpstream_demo539057668 \
				--metadataURL=https://metadata.playground.us-east-1.warpstream.com \
				--httpPort=8081 \
				--kafkaPort=9093 \
				--enableClusterWideEnvironment \
				--clusterWideEnvironmentPort=9081 \
				--gracefulShutdownDuration=0s
```

## Hello World

```shell
warpstream playground
```

Create topic:

```shell
warpstream kcmd --type create-topic --topic helloworld2
```

List topics:

```shell
kafka-topics --bootstrap-server localhost:9092 --list
```

Also create topic:

```shell
kafka-topics --bootstrap-server localhost:9092 --topic test-topic --create --partitions 3 --replication-factor 1
```

Produce Records:

```shell
warpstream kcmd --type produce --topic helloworld2 --records "world,,world"
```

Consume:

```shell
kafka-console-consumer --bootstrap-server localhost:9092 --topic helloworld2 --from-beginning --property print.timestamp=true --property print.key=true --property print.value=true
```

With warpstream:

```shell
warpstream kcmd --type fetch --topic helloworld2 --offset 0
```

Write to topic:

```shell
kafka-console-producer --broker-list localhost:9092 --topic helloworld2 --property parse.key=true --property key.separator=,
```

You can use something like:

```
test,testmessage
test2,another message
```

Read again.

Finally check APIs support:

```shell
kafka-broker-api-versions --bootstrap-server localhost:9092
```

# Minio

For running with a [local minIO](minio/README.md)