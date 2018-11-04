# openliberty-poc
Prototype using IBM OpenLiberty.

# Prerequisites

## Zipkin
If you want to see distributed traces, you will need to install Zipkin and start it up.  If it isn't running, the traces will disappear into the aether.  The easiest way to do this is to run the following commands.
```
curl -sSL https://zipkin.io/quickstart.sh | bash -s
java -jar zipkin.jar
```

Or you can run it via docker image:
```
docker run -d -p 9411:9411 openzipkin/zipkin
```

## Angular CLI
Without this, the front-end build will fail. To install:
```
cd ChuckWebApp
npm install @angular/cli
```

See https://angular.io/guide/quickstart for information about prerequisites

## Mountebank
The integration tests use Mountebank for over-the-wire test doubles so I don't anger the owner of the ICNDB application.  If you don't have mountebank running, the integration tests will fail.  For installation options see http://www.mbtest.org/docs/install.

Alternatively, you can run it via a docker image:
```
docker run -p 2525:2525 -d expert360/mountebank
```

# Instructions to run the Prototype
- Download and install the OpenLiberty server
  - I'm running Open Liberty 18.0.0.3 against OpenJDK 1.8.0_171
  - If you skip this step, it is possible for the start-liberty profile to install the liberty server to the openliberty.home directory for you, but you first have to remove the installDirectory attribute from the plugin configurations.  It is generally easier to install it yourself.
- Clone the repo
- Update the openliberty.home property in the poms
  - _TODO: this should be externalized_
- In each project run
  - mvn clean install -Pstart-liberty
    - This will create your server, configure it, and start it
    - If you change the server.xml or the bootstrap properties, you will need to rerun this and restart the liberty server
  - mvn clean install -Pdeploy
    - This will deploy the applications to the server apps directory
    - Once this is done, you can simply run mvn clean install for the backend and the server will pick up your latest changes
    - The frontend needs to be redeployed each time.  To understand why, look at the looseApplication parameter on the install-apps goal
- Alternatively to run using the fatjar concept
  - mvn clean install -Pfatjar
    - This will build a fatjar with an embedded openliberty container and start it directly from the console

# Things to look at

## The Chuck Application
https://localhost:8543/chuck

## Zipkin distributed trace logs
After you've accessed the application, you can head to http://localhost:9411/zipkin/ (if you started it up) to look at your traces.

## Try the HealthCheck
Healthcheck is available at https://localhost:8543/health

## Look at the metrics
Metrics are availabe at https://localhost:8443/metrics
You'll have to authenticate as an administrator using credentials admin:s3cret.
