# Building and Running the MicroServices container

## Build the container with the following command:

```cd base-image 
docker build -t msr-jdbc:10.15 .```

## Run the container:
```docker run -dp 6555:5555 --name msr -v /home/user/licenseKey.xml:/opt/softwareag/IntegrationServer/config/licenseKey.xml -e SAG_IS_LICENSE_FILE=/opt/softwareag/IntegrationServer/config/licenseKey.xml msr-jdbc:10.15```