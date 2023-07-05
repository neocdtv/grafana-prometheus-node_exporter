FROM azul/zulu-openjdk-alpine:11.0.9
USER root

RUN echo 'Installing packages...'
RUN apk add --no-cache curl tar bash procps
ARG MAVEN_VERSION=3.9.3
ARG USER_HOME_DIR="/root"
ARG BASE_URL=https://dlcdn.apache.org/maven/maven-3/${MAVEN_VERSION}/binaries

RUN mkdir -p /usr/share/maven /usr/share/maven/ref
RUN  echo "Downlaoding maven"
RUN curl -fsSL -o /tmp/apache-maven.tar.gz ${BASE_URL}/apache-maven-${MAVEN_VERSION}-bin.tar.gz
RUN echo "Unziping maven"
RUN tar -xzf /tmp/apache-maven.tar.gz -C /usr/share/maven --strip-components=1
RUN echo "Cleaning and setting links"
RUN rm -f /tmp/apache-maven.tar.gz
RUN ln -s /usr/share/maven/bin/mvn /usr/bin/mvn

ENV MAVEN_HOME /usr/share/maven

RUN mkdir /java_apps
COPY . java_app
RUN cd java_app && ./mvnw -T4 clean package
ENTRYPOINT ["java", "-jar", "java_app/target/quarkus-app/quarkus-run.jar"]
