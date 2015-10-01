#serialport 1.X not compatible with Node 4.0
FROM node:0.12.7
MAINTAINER Jose Antonio Hernandez Martinez <joherma1@gmail.com>

RUN mkdir -p /opt/siarest/src && \
    git clone --depth=1 -b develop https://github.com/joherma1/siarest.git /opt/siarest/src

RUN cd /opt/siarest/src && npm install

EXPOSE 3000
CMD ["node", "/opt/siarest/src/bin/www"]