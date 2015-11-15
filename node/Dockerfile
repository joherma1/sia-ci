FROM resin/raspberrypi-node
MAINTAINER Jose Antonio Hernandez Martinez <joherma1@gmail.com>

RUN mkdir -p /opt/siarest/src && \
    git clone --depth=1 https://github.com/joherma1/siarest.git /opt/siarest/src

RUN cd /opt/siarest/src && npm install

EXPOSE 3000
CMD ["node", "/opt/siarest/src/bin/www"]