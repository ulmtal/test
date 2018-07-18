FROM maven:latest AS app
WORKDIR /usr/src/test
COPY pom.xml .
RUN mvn -B -f pom.xml -s /usr/share/maven/ref/settings-docker.xml dependency:resolve
COPY . .
RUN mvn -B -s /usr/share/maven/ref/settings-docker.xml package -DskipTest

FROM tomee:8-jre-7.0.4-plume

# tomcat-users.xml sets up user accounts for the Tomcat manager GUI
# and script access for Maven deployments
WORKDIR /usr/local/tomee/
# copy and deploy application
WORKDIR /usr/local/tomee/webapps/
COPY --from=app /usr/src/test/target/test-0.0.1-SNAPSHOT.war .
# start tomcat7
EXPOSE 8080
CMD ["catalina.sh", "run"]