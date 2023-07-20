# Sample Business package for a CD catalog

## Setting up

To start the demo scenarion we will also be making use of the Oracle XE latest version. To be able to use the Oracle XE container, one must register itself on the oracle website and accept the license conditions on the [Container Registry](https://container-registry.oracle.com)

In order to get started run the following command:
```
docker run -d --name oracle -p 1521:1521 -p 5500:5500 container-registry.oracle.com/database/express:latest
```

Connect to the container and update the password for the system user:

```
docker exec -ti oracle /bin/sh

./setPassword.sh Oracle1.2.3
```

Next connect to the database and create a wmadmin user

```
sqlplus system/Oracle1.2.3@XEPDB1

create user cdcatalog identified by T0pS3cret quota unlimited on users;
grant connect,resource to cdcatalog;
```

## Create the Business package container

The Docker file is prepared to copy the linked CD Catalog into the base container created in the previous step. 
However, in a clean checkout the git submodule for this package has not been loaded. To pull the submodule run the following git command:

```
git submodule update
```

Now that the IS package is also loaded we can build the container.

```
docker build -t cd-catalog:1.0 .
```

Now we can run the container. Try to first check the contents of the application.properties file. If you start with the command below using the provided application properties, the JDBC Adapter will not be able to connect as the username & password are incorrect and the endpoint might not be right.

```
docker run --rm --name cd-catalog -v <your MicroservicesRuntime_100.xml license file>:/opt/softwareag/IntegrationServer/config/licenseKey.xml -e SAG_IS_LICENSE_FILE=/opt/softwareag/IntegrationServer/config/licenseKey.xml -dp 6555:5555 cd-catalog:1.0
```

## Generating configuration properties

Now that the container is running, we are able to access the container on port 6555. When logging in we can configure settings like the JDBC connection, in Security > User Management we can configer the LDAP connection and in UM can be configured in the Messaging configuration.
As this is not something we want to do each time the container starts we can (once configured) export the configuration as a file called application.properties. 
To do so navigate to Microservices > Configuration Variables. Here we can click Generate Configuration Variables Template which will be automatically downloaded. 
Now we the downloaded template, replace the values with the correct values for each environment or domain. To securely store password use the Generate Encrypted Configuration Varable on this page to generate an encrypted value one can put in the application.properties file. 

No that the file is correct start the container with the application properties:

```
docker run --rm --name cd-catalog -v <your MicroservicesRuntime_100.xml license file>:/opt/softwareag/IntegrationServer/config/licenseKey.xml -e SAG_IS_LICENSE_FILE=/opt/softwareag/IntegrationServer/config/licenseKey.xml -v `pwd`/application.properties:/opt/softwareag/IntegrationServer/application.properties -dp 6555:5555 cd-catalog:1.0
```