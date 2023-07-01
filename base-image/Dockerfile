FROM alpine:latest AS unzipper
RUN apk add unzip
COPY WmJDBCAdapter.zip /src/WmJDBCAdapter.zip
RUN mkdir /src/WmJDBCAdapter; \
  cd /src/WmJDBCAdapter; \
  unzip ../WmJDBCAdapter.zip

FROM sagcr.azurecr.io/webmethods-microservicesruntime:10.15
COPY  --from=unzipper --chown=sagadmin:sagadmin /src/WmJDBCAdapter /opt/softwareag/IntegrationServer/packages/WmJDBCAdapter