# Building and Running the MicroServices container

## Prerequisites
To get started we need to have the following assets available:
- zipfile of the WmJDBC Adapter
- database drivers for the database

To obtain the zipfile of the WmJDBC adapter, the easiest method is done through Designer. Do this by connecting to an IS that has the adapter installed, clicking the package, right click to select the option 'Export from server'.
Place the file WmJDBCAdapter.zip in this directory.

## Build the container with the following command:

```
cd base-image 
docker build -t msr-jdbc:10.15 .
```

## Run the container:
```
docker run -dp 6555:5555 --name msr -v /home/user/licenseKey.xml:/opt/softwareag/IntegrationServer/config/licenseKey.xml -e SAG_IS_LICENSE_FILE=/opt/softwareag/IntegrationServer/config/licenseKey.xml msr-jdbc:10.15
```