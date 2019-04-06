ARG FROM_DOCKER_IMAGE="guangie88/spark-custom"
ARG SPARK_VERSION=
ARG HADOOP_VERSION=
ARG HIVE_TAG_SUFFIX=
ARG PYSPARK_TAG_SUFFIX=
ARG DIST_TAG_SUFFIX=

FROM ${FROM_DOCKER_IMAGE}:${SPARK_VERSION}_hadoop-${HADOOP_VERSION}${HIVE_TAG_SUFFIX}${PYSPARK_TAG_SUFFIX}${DIST_TAG_SUFFIX}

ARG HADOOP_VERSION=
ARG AWS_JAVA_SDK_VERSION=

RUN set -euo pipefail && \
    # apt requirements
    apk add --no-cache \
        curl \
        ; \
    # AWS S3 JAR
    cd ${SPARK_HOME}/jars; \
    curl -LO http://central.maven.org/maven2/org/apache/hadoop/hadoop-aws/${HADOOP_VERSION}/hadoop-aws-${HADOOP_VERSION}.jar; \
    curl -LO https://sdk-for-java.amazonwebservices.com/aws-java-sdk-${AWS_JAVA_SDK_VERSION}.zip; \
        unzip aws-java-sdk-${AWS_JAVA_SDK_VERSION}.zip; \
        find aws-java-sdk-${AWS_JAVA_SDK_VERSION} -name "*.jar" -exec mv {} ${SPARK_HOME}/jars/ \; ; \
        rm -r ./aws-java-sdk-${AWS_JAVA_SDK_VERSION}; \
        rm ./aws-java-sdk-${AWS_JAVA_SDK_VERSION}.zip; \
    echo "spark.hadoop.fs.s3a.impl    org.apache.hadoop.fs.s3a.S3AFileSystem" >> ${SPARK_HOME}/conf/spark-defaults.conf; \
    # Google Storage JAR
    curl -LO https://storage.googleapis.com/hadoop-lib/gcs/gcs-connector-hadoop2-latest.jar; \
    # MariaDB connector JAR
    curl -LO https://downloads.mariadb.com/Connectors/java/connector-java-2.4.0/mariadb-java-client-2.4.0.jar; \
    cd -; \
    # Required for prevent snappy compression error because the shared lib is dependent on glibc
    apk add --no-cache gcompat libc6-compat; \
    # apt clean-up
    apk del \
        curl \
        ; \
    rm -rf /var/lib/apt/lists/*; \
    :