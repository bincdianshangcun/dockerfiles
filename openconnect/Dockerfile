FROM gibby/base:debian8
MAINTAINER Chris Gibson <git@twoitguys.com>

RUN apt-get update && \
    apt-get install -y openconnect netcat-traditional ocproxy dnsutils telnet unbound gettext polipo && \
    apt-get install -y iptables clamav clamav-daemon && \
    apt-get clean && \
    rm -rf /var/cache/apt/* && \
    rm -rf /var/lib/apt/lists/*


COPY iptables.up.rules /etc/iptables.up.rules
COPY iptables.up /etc/network/if-pre-up.d/iptables


COPY run.sh /run.sh
RUN chmod 0755 /run.sh

CMD ["/run.sh"]
