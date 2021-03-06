# What is this?
A container that runs unbound and openconnect and exposes a SOCKS5 proxy and HTTP proxy.
NOTE: Privileged mode is needed for the TUN adapter in the container.

Note: After unbound starts up(if UNBOUND is set to true), it overwrites /etc/resolv.conf in the container to use 127.0.0.1

## Environment variables needed
PASSWORD - The password passed to openconnect.
OPTIONS - Options passed to openconnect. Example -e OPTIONS="-u awesome_admin --authgroup=ADMINS --no-cert-check"
SERVER - Server to connect to with openconnect.


## Optional Environment variables needed
UNBOUND - true/false (Default:false, to run unbound in the container also or not)

HTTP_PROXY_USERNAME - Username for the HTTP proxy
HTTP_PROXY_PASSWORD - Password for the HTTP proxy

Note: Both HTTP_PROXY_USERNAME and HTTP_PROXY_PASSWORD need to be specified for the HTTP proxy to be exposed with authentication.

## File mount
/etc/unbound/unbound.conf for Unbound.

## Docker run options needed
And a port for the proxies
-p 127.0.0.1:9000:9000
-p 127.0.0.1:8123:8123

## Example 1 password / HTTP Proxy Auth
docker run --privileged \

-p 127.0.0.1:9000:9000 \

-p 127.0.0.1:8123:8123 \

-v custom.conf:/etc/unbound/unbound.conf.d/custom.conf \

-e HTTP_PROXY_USERNAME="foo" \

-e HTTP_PROXY_PASSWORD="bar" \

-e OPTIONS="-u awesome_admin --authgroup=ADMINS --no-cert-check" \

-e SERVER=vpnserver.local \

-e PASSWORD=changeme\n \

-t gibby/openconnect

## Example 2 passwords (like 2FA) / No HTTP Proxy Auth
docker run --privileged \

-p 127.0.0.1:9000:9000 \

-p 127.0.0.1:8123:8123 \

-v custom.conf:/etc/unbound/unbound.conf.d/custom.conf \

-e OPTIONS="-u awesome_admin --authgroup=ADMINS --no-cert-check" \

-e SERVER=vpnserver.local \

-e PASSWORD=changeme\n 123456\n \

-t gibby/openconnect

## To also expose Unbound (DNS) add
-p 127.0.0.1:53:53/tcp \
-p 127.0.0.1:53:53/udp \

## SSH Config on localhost
Put something like below in your .ssh/config with ProxyCommand for the hosts behind the vpn

ProxyCommand ssh -p 9000 root@localhost nc %h %p


## TODO
Add instructions for using redsocks on linux with iptables to route networks through iptables to redsocks to the socks 5 proxy on the container
