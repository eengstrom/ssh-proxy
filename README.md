# ssh-proxy
ssh ProxyCommand wrapper script to smartly proxy when necessary.

Simple shell wrapper to proxy/tunnel or hop through ssh when unable
to connect directly.  Often useful on laptops, especially when only
SOMETIMES behind a draconian firewall or outside a network with
limited ssh connections inbound.

Intended for use inside an ssh-config file, something like this
for an outbound http proxy (behind a restrictive outbound firewall):

    Host *
      ProxyCommand $HOME/.ssh/proxy -p httpproxy.foo.com:8080 %h %p

or, inbound hop, when limited inbound ssh connections allowed.

    Host *
      ProxyCommand $HOME/.ssh/proxy -h ssh-hop.foo.com:8080 %h %p

In either case, the script will first attempt a direct connection,
avoiding any overhead.  In all cases, if you have a VPN option, that
will probably be better, but even in those cases, this script should
not interfere.
