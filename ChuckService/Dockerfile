FROM maven:3.5.2-jdk-8-alpine AS MavenBuild
RUN ["apk", "update"]
RUN ["apk", "add", "openssl"]
RUN ["wget", "-P", ".", "https://public.dhe.ibm.com/ibmdl/export/pub/software/openliberty/runtime/release/2018-09-05_2337/openliberty-18.0.0.3.zip"]
RUN ["mkdir", "-p", "/opt/"]
RUN ["unzip", "openliberty-18.0.0.3.zip", "-d", "/opt/"]
COPY pom.xml /tmp/
COPY src /tmp/src
WORKDIR /tmp/
RUN ["mvn", "install"]

FROM openjdk:8u181-alpine
RUN ["mkdir", "-p", "/opt/chuck"]
WORKDIR /opt/chuck/
COPY --from=MavenBuild /tmp/target/chuck-service.jar .
ENTRYPOINT ["java", "-jar", "./chuck-service.jar"]