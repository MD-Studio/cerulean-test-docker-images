FROM mdstudio/cerulean-test-base:latest

USER root

RUN apt-get update && \
    apt-get --no-install-recommends -y install tar gcc g++ make \
        pkg-config libssl-dev libxml2-dev libboost-dev wget && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN mkdir /build
WORKDIR /build
RUN wget -nv http://www.adaptivecomputing.com/index.php?wpfb_dl=3244 -O torque-6.1.2.tar.gz && \
    tar xf torque-6.1.2.tar.gz

WORKDIR /build/torque-6.1.2
RUN ./configure >/tmp/configure.log 2>&1 && \
    tail -n 20 /tmp/configure.log && \
    make >/tmp/make.log 2>&1 && \
    tail -n 20 /tmp/make.log && \
    make install >/tmp/make_install.log 2>&1 && \
    tail -n 20 /tmp/make_install.log

COPY start-services.sh /etc/start-services.sh
RUN chmod +x /etc/start-services.sh
CMD /etc/start-services.sh

USER cerulean

HEALTHCHECK --interval=1s CMD ssh -o NoHostAuthenticationForLocalhost=yes -i /home/cerulean/.ssh/id1_rsa cerulean@localhost /bin/true && pbsnodes -l free $(hostname) || exit 1