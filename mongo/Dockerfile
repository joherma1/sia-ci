FROM resin/rpi-buildpack-deps:jessie
MAINTAINER Jose Antonio Hernandez Martinez <joherma1@gmail.com>

RUN apt-get update && apt-get install -y mongodb-server
RUN mkdir /usr/src/data

EXPOSE 27017 27018
CMD ["mongod", "--dbpath", "/usr/src/data"]
