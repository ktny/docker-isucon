FROM golang:1.9

ARG GIT_URL

RUN git clone $GIT_URL /home/isucon/isubata
WORKDIR /home/isucon/isubata/webapp/go
RUN make

EXPOSE 5000
EXPOSE 19999

ENTRYPOINT ["/home/isucon/isubata/webapp/go/isubata"]
