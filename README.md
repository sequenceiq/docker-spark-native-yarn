Apache Spark on native YARN in Docker
==========

This repository is a `containerized` version of the [spark-native-yarn](https://github.com/hortonworks/spark-native-yarn) project - an improvement for Apache Spark by allowing pluggable execution contexts introduced with the [SPARK-3561](https://issues.apache.org/jira/browse/SPARK-3561) JIRA. The new execution context is `Apache Tez`.

This Docker image depends on our previous [Hadoop Docker](https://github.com/sequenceiq/hadoop-docker) image, available at the SequenceIQ [GitHub](https://github.com/sequenceiq) page. The base Hadoop Docker image is also available as an official [Docker image](https://registry.hub.docker.com/u/sequenceiq/hadoop-docker/) (sequenceiq/hadoop-docker).

##Pull the image from Docker Repository
```
docker pull sequenceiq/spark-native-yarn
```

## DIY - Building the image
```
docker build --rm -t sequenceiq/spark-native-yarn
```

## Running the image
```
docker run -i -t -h sandbox sequenceiq/spark-native-yarn /etc/bootstrap.sh -bash
```

## Versions
```
Hadoop 2.5.1 and Apache Spark 1.1.0 and Apache Tez 0.5

```

##Testing

After you have run the image you are ready to start playing wint Spark on YARN, an using Tez as the execution context.

```
cd /usr/local/spark
./bin/spark-submit --class org.apache.spark.examples.SparkPi --master execution-context:org.apache.spark.tez.TezJobExecutionContext --conf update-classpath=true ./lib/spark-examples-1.1.0.2.1.5.0-702-hadoop2.4.0.2.1.5.0-695.jar
```

