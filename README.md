Apache Spark on native YARN in Docker
==========

This repository is a `containerized` version of the [spark-native-yarn](https://github.com/hortonworks/spark-native-yarn) project - an improvement for Apache Spark by allowing pluggable execution contexts introduced with the [SPARK-3561](https://issues.apache.org/jira/browse/SPARK-3561) JIRA. The new execution context is `Apache Tez`.

This Docker image depends on our previous [Hadoop Docker](https://github.com/sequenceiq/hadoop-docker) image, available at the SequenceIQ [GitHub](https://github.com/sequenceiq) page. The base Hadoop Docker image is also available as an official [Docker image](https://registry.hub.docker.com/u/sequenceiq/hadoop-docker/) (sequenceiq/hadoop-docker).


###Pull the image from the Docker Repository

We suggest to always pull the container from the official Docker repository - as this is always maintained and supported by us.

```
docker pull sequenceiq/spark-native-yarn
```

Once you have pulled the container you are ready to run the image.

###Run the image

```
docker run -i -t -h sandbox sequenceiq/spark-native-yarn /etc/bootstrap.sh -bash
```

###Versions

```
Hadoop 2.5.1 and Apache Spark 1.1.0 and Apache Tez 0.5
```

You now have a fully configured Apache Spark, where the `execution context` is [Apache Tez](http://tez.apache.org/).

###Test the container

We have pushed sample data and tests from the [code repository](https://github.com/hortonworks/spark-native-yarn-samples) into the Docker container, thus you can start experimenting right away without writing one line of code.

####Calculate PI
Simplest example to test with is the `PI calculation`.

```
cd /usr/local/spark
./bin/spark-submit --class org.apache.spark.examples.SparkPi --master execution-context:org.apache.spark.tez.TezJobExecutionContext --conf update-classpath=true ./lib/spark-examples-1.1.0.2.1.5.0-702-hadoop2.4.0.2.1.5.0-695.jar
```

You should expect something like the following as the result:
```
Pi is roughly 3.14668
```

####Run a KMeans example

Run the `KMeans` example using the sample dataset.

```
./bin/spark-submit --class sample.KMeans --master execution-context:org.apache.spark.tez.TezJobExecutionContext --conf update-classpath=true ./lib/spark-native-yarn-samples-1.0.jar /sample-data/kmeans_data.txt
```

You should expect something like the following as the result:
```
Finished iteration (delta = 0.0)
Final centers:
DenseVector(0.15000000000000002, 0.15000000000000002, 0.15000000000000002)
DenseVector(9.2, 9.2, 9.2)
DenseVector(0.0, 0.0, 0.0)
DenseVector(9.05, 9.05, 9.05)
```
####Other examples (Join, Partition By, Source count, Word count)

Join
```
./bin/spark-submit --class sample.Join --master execution-context:org.apache.spark.tez.TezJobExecutionContext --conf update-classpath=true ./lib/spark-native-yarn-samples-1.0.jar /sample-data/join1.txt /sample-data/join2.txt
```
Partition By
```
./bin/spark-submit --class sample.PartitionBy --master execution-context:org.apache.spark.tez.TezJobExecutionContext --conf update-classpath=true ./lib/spark-native-yarn-samples-1.0.jar /sample-data/partitioning.txt
```
Source count
```
./bin/spark-submit --class sample.SourceCount --master execution-context:org.apache.spark.tez.TezJobExecutionContext --conf update-classpath=true ./lib/spark-native-yarn-samples-1.0.jar /sample-data/wordcount.txt
```
Word count
```
./bin/spark-submit --class sample.WordCount --master execution-context:org.apache.spark.tez.TezJobExecutionContext --conf update-classpath=true ./lib/spark-native-yarn-samples-1.0.jar /sample-data/wordcount.txt 1
```
Note that the last argument (1) is the number of `reducers`.

###Using the Spark Shell

The Spark shell works out of the box with the new Tez `executor context`, the only thing you will need to do is run:

```
./bin/spark-shell --master execution-context:org.apache.spark.tez.TezJobExecutionContext
```
