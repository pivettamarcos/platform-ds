ARG DEBIAN_BUSTER_IMAGE_TAG=8-jre-slim
FROM openjdk:${DEBIAN_BUSTER_IMAGE_TAG}

ENV DAEMON_RUN=true
ENV SPARK_VERSION=3.5.0
ENV HADOOP_VERSION=3

# Install Java and some OS dependencies to monitor the container
RUN apt-get update && apt-get install -y --no-install-recommends \
    apt-transport-https ca-certificates \
    curl \
    python3 \
    pkg-config \
    wget \
    rsync \
    software-properties-common \
    unzip \
    logrotate \
    supervisor \
    net-tools \
    && \
    ln -s /usr/bin/python3 /usr/bin/python && \
    apt-get clean

#ENV JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64

# Configure Supervisor to display web interface
RUN rm /etc/supervisor/supervisord.conf
ADD supervisor/supervisord.conf /etc/supervisor/
    
# Install Spark
RUN wget --no-verbose https://archive.apache.org/dist/spark/spark-${SPARK_VERSION}/spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz && tar -xvzf spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz \
      && mv spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION} /opt/spark \
      && rm spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz

ENV http_proxy=${http_proxy}
ENV https_proxy=${https_proxy}
