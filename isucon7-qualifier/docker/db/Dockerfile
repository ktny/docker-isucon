FROM golang:1.9 AS build-dev

ARG GIT_URL

RUN \
  git clone --depth=1 $GIT_URL /home/isucon/isubata && \
  cd /home/isucon/isubata/bench && \
  go get github.com/constabulary/gb/... && \
  gb vendor restore && \
  make && \
  bin/gen-initial-dataset

FROM mysql:5.7

ENV MYSQL_ALLOW_EMPTY_PASSWORD=yes MYSQL_DATABASE=isubata MYSQL_USER=isucon MYSQL_PASSWORD=isucon

RUN apt update && \
  apt install -y curl less && \
  curl -LO https://percona.com/get/pt-query-digest && \
  chmod +x pt-query-digest && \
  mv pt-query-digest /usr/local/bin/

COPY --from=build-dev /home/isucon/isubata/db/isubata.sql /docker-entrypoint-initdb.d/01_isubata.sql
COPY --from=build-dev /home/isucon/isubata/bench/isucon7q-initial-dataset.sql.gz /docker-entrypoint-initdb.d/isucon7q-initial-dataset.sql.gz 
COPY my.cnf /etc/mysql/conf.d/isucon-my.cnf
RUN chmod 644 /etc/mysql/conf.d/isucon-my.cnf

CMD ["--character-set-server=utf8mb4"]
