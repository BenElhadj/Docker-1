# docker build -t test00 .
# docker run -it test00
FROM risingstack/alpine:3.3-v4.2.6-1.1.3
MAINTAINER bhamdi <bhamdi@student.42.fr>
WORKDIR /opt/app

RUN apk update
RUN apk add vim

CMD ["vim", "test00"]
