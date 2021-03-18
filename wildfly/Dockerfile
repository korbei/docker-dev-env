FROM jboss/wildfly

ARG jdbc_driver

RUN echo "------jdbc-driver: $jdbc_driver"

ENV DB_HOST postgres
ENV DB_PORT 5432
ENV DB_NAME myDB
ENV DB_USER postgres
ENV DB_PASS postgres

ENV DS_NAME myDatasource
ENV JNDI_NAME java:/jdbc/$DS_NAME

USER root

ADD https://jdbc.postgresql.org/download/postgresql-42.2.5.jar /tmp/postgresql-42.2.5.jar
ADD https://repo1.maven.org/maven2/mysql/mysql-connector-java/8.0.17/mysql-connector-java-8.0.17.jar /tmp/mysql-connector-java-8.0.17.jar
ADD https://repo1.maven.org/maven2/com/oracle/ojdbc/orai18n/19.3.0.0/orai18n-19.3.0.0.jar /tmp/orai18n-19.3.0.0.jar
ADD https://repo1.maven.org/maven2/com/oracle/database/jdbc/ojdbc8/19.7.0.0/ojdbc8-19.7.0.0.jar /tmp/ojdbc8-19.7.0.0.jar
ADD scripts/install-postgres-module.cli /tmp/install-postgres-module.cli
ADD scripts/install-mysql-module.cli /tmp/install-mysql-module.cli
ADD scripts/install-oracle-module.cli /tmp/install-oracle-module.cli

RUN /bin/sh -c '$JBOSS_HOME/bin/standalone.sh &' && \
  sleep 10 && \
  $JBOSS_HOME/bin/jboss-cli.sh --connect --file=/tmp/install-${jdbc_driver}-module.cli && \
  $JBOSS_HOME/bin/jboss-cli.sh --connect --command=:shutdown && \
  rm -rf $JBOSS_HOME/standalone/configuration/standalone_xml_history/ $JBOSS_HOME/standalone/log/*

RUN ${JBOSS_HOME}/bin/add-user.sh admin admin --silent

CMD ["/opt/jboss/wildfly/bin/standalone.sh", "-b", "0.0.0.0", "-bmanagement", "0.0.0.0"]
