# php-oci8-alpine

Cara install oci8 PHP extension dalam alpine linux

Skrip dalam gitlab-ci.yml

```
- LD_LIBRARY_PATH /usr/local/instantclient
- ORACLE_HOME /usr/local/instantclient
# Install Oracle Client and build OCI8 (Oracle Command Interface 8 - PHP extension)
- apk add php7-pear php7-dev gcc musl-dev libnsl libaio &&\
## Download and unarchive Instant Client v12.2
- curl -o /tmp/basic.zip https://gist.github.com/ApOgEE/0680abf55f0e8d724870225cd5e11bc1/raw/3d7fa5d91a66c0c2279a9d397ec9e3e28dcca99a/instantclient-basic-linux.x64-12.2.0.1.0.zip && \
- curl -o /tmp/sdk.zip https://raw.githubusercontent.com/bumpx/oracle-instantclient/master/instantclient-sdk-linux.x64-11.2.0.4.0.zip && \
- curl -o /tmp/sqlplus.zip https://raw.githubusercontent.com/bumpx/oracle-instantclient/master/instantclient-sqlplus-linux.x64-11.2.0.4.0.zip && \
- unzip -d /usr/local/ /tmp/basic.zip && \
- unzip -d /usr/local/ /tmp/sdk.zip && \
- unzip -d /usr/local/ /tmp/sqlplus.zip && \
## Links are required for older SDKs
- ln -s /usr/local/instantclient_12_2 ${ORACLE_HOME} && \
- ln -s ${ORACLE_HOME}/libclntsh.so.* ${ORACLE_HOME}/libclntsh.so && \
- ln -s ${ORACLE_HOME}/libocci.so.* ${ORACLE_HOME}/libocci.so && \
- ln -s ${ORACLE_HOME}/lib* /usr/lib && \
- ln -s ${ORACLE_HOME}/sqlplus /usr/bin/sqlplus &&\
- ln -s /usr/lib/libnsl.so.2.0.0  /usr/lib/libnsl.so.1 &&\
## Build OCI8 with PECL
- echo "instantclient,${ORACLE_HOME}" | pecl install oci8 &&\
- echo 'extension=oci8.so' > /etc/php7/conf.d/30-oci8.ini &&\
#  Clean up
- apk del php7-pear php7-dev gcc musl-dev &&\
- rm -rf /tmp/*.zip /var/cache/apk/* /tmp/pear/
```
