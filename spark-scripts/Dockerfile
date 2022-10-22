
FROM apache/spark-py:latest

ARG spark_uid=185

# Before building the docker image, first build and make a Spark distribution following
# the instructions in https://spark.apache.org/docs/latest/building-spark.html.
# If this docker file is being used in the context of building your images from a Spark
# distribution, the docker build command should be invoked from the top level directory
# of the Spark distribution. E.g.:
# docker build -t spark:latest -f kubernetes/dockerfiles/spark/Dockerfile .
USER root

RUN set -ex && \
    apt-get update && \
    apt install -yqq openssh-client

ENV SPARK_HOME /opt/spark

WORKDIR /opt/spark/
RUN chmod g+w /opt/spark/work-dir
RUN chmod a+x /opt/decom.sh

COPY /spark-scripts/spark-env.sh /opt/spark/conf/spark-env.sh
COPY /spark-scripts/entrypoint.sh /opt/entrypoint.sh
RUN chmod 777 /opt/spark/conf/spark-env.sh
RUN chmod a+x /opt/entrypoint.sh

ENV PYSPARK_PYTHON=/usr/bin/python3 \
    PYSPARK_DRIVER_PYTHON=/usr/bin/python3 \
    SPARK_MASTER_HOST=py-spark \
    PATH="${PATH}:${SPARK_HOME}/bin" \
    PATH="${PATH}:${SPARK_HOME}/sbin"



# Specify the User that the actual main process will run as
USER root
