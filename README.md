# ssh-proxy
ssh ProxyCommand wrapper script to smartly proxy when necessary.

Simple shell wrapper to proxy/tunnel or hop through ssh when unable
to connect directly.  Often useful on laptops, especially when only
SOMETIMES behind a draconian firewall or outside a network with
limited ssh connections inbound.

Intended for use inside an ssh-config file, something like this:

    Host *
      ProxyCommand $HOME/.ssh/proxy -p httpproxy.foo.com:8080 %h %p
      
or 

      ProxyCommand $HOME/.ssh/proxy -h ssh-hop.foo.com:8080 %h %p
