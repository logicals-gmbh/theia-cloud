FROM maven:3.8.4-openjdk-11 AS builder
WORKDIR /operator
COPY java/common ./common
COPY java/operator ./operator
RUN cd /operator/common/maven-conf && \
    mvn clean install && \
    cd /operator/common/org.eclipse.theia.cloud.common && \
    mvn clean install && \
    cd /operator/operator/org.eclipse.theia.cloud.operator && \
    mvn clean verify

FROM openjdk:11-jre-slim-buster
RUN mkdir /templates
WORKDIR /log-config
COPY java/operator/org.eclipse.theia.cloud.operator/log4j2.xml .
WORKDIR /operator
COPY --from=builder /operator/operator/org.eclipse.theia.cloud.operator/target/operator-0.7.0-SNAPSHOT-jar-with-dependencies.jar .
ENTRYPOINT [ "java", "-Dlog4j2.configurationFile=/log-config/log4j2.xml", "-jar", "./operator-0.7.0-SNAPSHOT-jar-with-dependencies.jar" ]
CMD [ "" ]