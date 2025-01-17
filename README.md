# platform-ds

Datascience environment managed by Docker and Docker-compose. This stack creates a standalone Spark cluster with 2 workers, 1 master and 1 driver (jupyterlab). It also creates a hadoop yarn cluster with: 1 resource manager; 1 node manager with 1 namenode, 1 data node; a history server.

### Prerequisites

- [Docker](https://www.docker.com)
- [Docker-compose](https://docs.docker.com/compose/)

### Launch the platform

```sh
$ git clone <repo_url>
$ cd plateforme-ds
$ make
```
Then, if you want to start a spark-cluster

```sh
$ docker-compose -f spark-local.yml up -d
```

Or a spark on local jupyter container

```sh
$ docker-compose -f spark-cluster.yml up -d
```

You can access namenode container by running the following command:

```sh
$ docker exec -it namenode bash
```
You can access jupyter container to obtain the token key by running the following command:

```sh
$ docker exec -it jupyter bash
$ jupyter notebook list
```

### Spark and Hadoop in jupyter

If you launch the spark cluster, you can connect to it in the jupyter notebook by running the following code:

```py
from pyspark import SparkConf, SparkContext

conf = SparkConf().setAppName('test').setMaster('spark://spark-master:7077')
sc = SparkContext(conf=conf)
```

And read files from HDFS system as follow:

```py
lines = sc.textFile("hdfs://namenode:9000/<your_path_to_the_file>")
```

### Connect to the platform

- go to the url http://<ip_or_hostname_server>:10000 to open a jupyterlab session
- Hadoop nanemode: http://<ip_or_hostname_server>:9870
- Hadoop datanode: http://<ip_or_hostname_server>:9864
- Ressource Manager: http://<ip_or_hostname_server>:8088

*Spark cluster*
- Spark master: http://<ip_or_hostname_server>:8585 (webui) or http://<ip_or_hostname_server>:7077 (jobs)
- Spark worker-[x]: http://<ip_or_hostname_server>:808[x]

*Spark local*
- Spark webui: http://<ip_or_hostname_server>:4040

### TODO LIST

- Add linked folder between jupyter container and host machine (handle permission issues)

### Quick Start HDFS:
- Copy breweries.csv to the namenode.
  docker cp data/1/cardio_base.csv namenode:cardio_base.csv
- Go to the bash shell on the namenode with that same Container ID of the namenode.
  docker exec -it namenode bash
- Create a HDFS directory /data/.
  hdfs dfs -mkdir -p /data/
- Copy cardio_base.csv to HDFS:
  hdfs dfs -put cardio_base.csv /data/cardio_base.csv
