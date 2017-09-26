Standalone Mule ESB Enterprise Docker Image
===============

This project contains a Dockerfile for the deployment and packaging of a standalone Mule ESB Enterprise with Docker.

Preparing the Docker base image
---------------

Due to restrictions of the Enterprise version, the Docker image needs to be set up for individual usage beforehand. It is required to:
- provide the Enterprise standalone server from Mulesoft. In this Dockerfile we asume the downloaded trial version from [mulesoft.com](http://www.mulesoft.com/mule-esb-enterprise-30-day-trial), located in the same folder as the Dockerfile.
- provide the Enterprise license file from Mulesoft, located in the same folder as the Dockerfile.

Hence the directory should look like this:
* mule-ee/
* mule-ee/Dockerfile
* mule-ee/start.sh


Building and tagging the Docker base image
---------------

```bash
wget -c -t 0  https://s3.amazonaws.com/new-mule-artifacts/mule-ee-distribution-standalone-3.8.3.zip
docker build --tag="thomas-li/mule-ee" .
```

Using the Docker image
---------------

You can create an Docker image for each Mule ESB application to isolate applications from each other and this way startup and create multiple standalone Mule ESB instances.

Standalone Mule ESB Enterprise Container
---------------

Start a standalone Mule ESB Enterprise instance

```bash
docker run -t -i --name='mule-ee-instance-X' thomas-li/mule-ee
```


App specific container image
---------------

```bash
FROM                    thomas-li/mule-ee:latest
.
.
ADD                     mule-app/target/mule-app-1.0.0-SNAPSHOT.zip /opt/mule/apps/
```

Build application specific Docker image:

```bash
docker build --tag="my-mule-app-image" .
```

Start app specific image:

```bash
docker run -t -i --name='mule-app-node' my-mule-app-image
for example (including externally mountable apps directory):
docker run -it --name='mule-ee' -v `pwd`/apps:/opt/mule/apps thomas-li/mule-ee
```