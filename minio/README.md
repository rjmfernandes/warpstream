# WarpStream with MinIO

First create an account in [WarpStream](https://console.warpstream.com/login).

After create a cluster give it any name you want. Keep control plane region as `us-east-1` for keeping things simple for now. You can edit the `compose.yml` to use a different region later on.

After create a local file named `.env`with the contents:

```
AGENT_KEY=aks_219b1fd9cc4d64ed28a9b02346f8bc44f0f2ce119b482a2XXXX
VIRTUAL_CLUSTER_ID=vci_0170c152_aaaa_bbbb_cccc_efeb87618efd
```

Replace the values in it with the ones corresponding to your just created cluster. You can find the `Cluster ID` on the `Overview` tab. And the Agent `Key` under the `Agent Keys` tab.

Now you can execute (make sure you are on minio folder):

```shell
docker compose up -d
```

Refresh the `Overview` page for your cluster in WarpStream and you should see your `localhost` agent running.

Let's create a topic:

```shell
kafka-topics --bootstrap-server localhost:9092 --topic test --create --partitions 3 --replication-factor 1
```

Under the `Topics` page of WarpStream again you should see the new topic `test` listed with 3 partitions.

Let's write into it:

```shell
kafka-console-producer --broker-list localhost:9092 --topic test --property parse.key=true --property key.separator=,
```

For example:

```
test,testmessage
test2,another message
```

You should see a small peak of about 1 bytes/s on WarpStream for the throughput.

Check your MinIO (http://localhost:9090 with user/password being `admin`/`password`) and for your bucket `warpstream-minio-bucket` you should see your data.

Consume:

```shell
kafka-console-consumer --bootstrap-server localhost:9092 --topic test --from-beginning --property print.timestamp=true --property print.key=true --property print.value=true
```

If you click on the topic in WarpStream interface you should be able to see the max offset per partition.

## Clean up

```shell
docker compose down -v
rm -fr ./data
```

Delete your cluster in WarpStream.







