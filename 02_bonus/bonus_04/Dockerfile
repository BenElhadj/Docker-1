# docker-machine create --driver virtualbox yarn-demo
# eval $(docker-machine env yarn-demo)
# sh build.sh
# docker build . -t yarn-demo
# docker run yarn-demo node -e "console.log('Hello, World')"

FROM risingstack/alpine:3.4-v6.7.0-4.0.0

MAINTAINER bhamdi <bhamdi@student.42.fr>

WORKDIR /opt/app

# Install yarn
RUN mkdir -p /opt
ADD latest.tar.gz /opt/
RUN mv /opt/dist /opt/yarn
ENV PATH "$PATH:/opt/yarn/bin"

ADD package.json yarn.lock /tmp/

# Copy cache contents (if any) from local machine
ADD .yarn-cache.tgz /

# Install packages
RUN cd /tmp && yarn
RUN mkdir -p /opt/app && cd /opt/app && ln -s /tmp/node_modules

# Copy the code
ADD . /opt/app
