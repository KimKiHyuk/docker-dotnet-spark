ARG DOTNET_SPARK_VERSION=1.0.0
FROM dotnet-spark-base-interactive:$DOTNET_SPARK_VERSION

ARG SPARK_VERSION=3.0.1
ARG DOTNET_SPARK_JAR="microsoft-spark-3-0_2.12-$DOTNET_SPARK_VERSION"

ENV DAEMON_RUN=true
ENV SPARK_VERSION=$SPARK_VERSION
ENV SPARK_HOME=/spark

ENV HADOOP_VERSION=2.7
ENV PATH="${SPARK_HOME}/bin:${DOTNET_WORKER_DIR}:${PATH}"
ENV DOTNETBACKEND_PORT=5567
ENV JUPYTER_ENABLE_LAB=true

USER root

COPY bin/* /usr/local/bin/

COPY *.ipynb ${HOME}/dotnet.spark/examples/

RUN cd / \
    && wget -q --show-progress --progress=bar:force:noscroll https://archive.apache.org/dist/spark/spark-${SPARK_VERSION}/spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz \
    && tar -xvzf spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz \
    && mv spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION} spark \
    && rm spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz \
    && chmod 755 /usr/local/bin/start-spark-debug.sh \
    && cp /dotnet/Debug/netcoreapp${DOTNET_CORE_VERSION}/${DOTNET_SPARK_JAR} ${HOME}/ \
    && chown -R ${NB_UID} ${HOME}

USER ${NB_USER}
WORKDIR ${HOME}/dotnet.spark