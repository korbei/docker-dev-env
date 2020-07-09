FROM jboss/wildfly

ENV DB_HOST postgres
ENV DB_PORT 5432
ENV DB_NAME myDB
ENV DB_USER postgres
ENV DB_PASS postgres

ENV DS_NAME myDatasource
ENV JNDI_NAME java:/jdbc/$DS_NAME

USER root

ADD https://jdbc.postgresql.org/download/postgresql-42.2.5.jar /tmp/postgresql.jar
ADD scripts/install-postgres-module.cli /tmp/install-postgres-module.cli

RUN /bin/sh -c '$JBOSS_HOME/bin/standalone.sh &' && \
  sleep 10 && \
  $JBOSS_HOME/bin/jboss-cli.sh --connect --file=/tmp/install-postgres-module.cli && \ 
  $JBOSS_HOME/bin/jboss-cli.sh --connect --command=:shutdown && \
  rm -rf $JBOSS_HOME/standalone/configuration/standalone_xml_history/ $JBOSS_HOME/standalone/log/*

RUN ${JBOSS_HOME}/bin/add-user.sh admin admin --silent

CMD ["/opt/jboss/wildfly/bin/standalone.sh", "-b", "0.0.0.0", "-bmanagement", "0.0.0.0"]