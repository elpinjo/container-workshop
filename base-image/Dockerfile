FROM alpine:latest AS unzipper
RUN apk add unzip
COPY WmJDBCAdapter.zip /src/WmJDBCAdapter.zip
ADD https://download.oracle.com/otn-pub/otn_software/jdbc/232-DeveloperRel/ojdbc11.jar /src/ojdbc11.jar
RUN mkdir /src/WmJDBCAdapter; \
  cd /src/WmJDBCAdapter; \
  unzip ../WmJDBCAdapter.zip; \
  cd /src; cp ojdbc11.jar WmJDBCAdapter/code/jars

FROM sagcr.azurecr.io/webmethods-microservicesruntime:10.15
COPY  --from=unzipper --chown=sagadmin:sagadmin /src/WmJDBCAdapter /opt/softwareag/IntegrationServer/packages/WmJDBCAdapter
COPY --from=unzipper --chown=sagadmin:sagadmin /src/ojdbc11.jar /opt/softwareag/IntegrationServer/lib/jars