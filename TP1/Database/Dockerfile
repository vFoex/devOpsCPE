FROM postgres:14.1-alpine

ENV POSTGRES_DB=db POSTGRES_USER=usr POSTGRES_PASSWORD=pwd

ADD CreateScheme.sql /docker-entrypoint-initdb.d
ADD InsertData.sql /docker-entrypoint-initdb.d