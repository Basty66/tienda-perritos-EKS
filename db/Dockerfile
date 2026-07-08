FROM mysql:8

USER mysql
EXPOSE 3306
COPY init.sql /docker-entrypoint-initdb.d/

HEALTHCHECK --interval=30s --timeout=5s --start-period=30s --retries=5 \
  CMD mysqladmin ping -h localhost -u root -p$MYSQL_ROOT_PASSWORD || exit 1
