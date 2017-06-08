# ssh-proxy
ssh ProxyCommand script to smartly proxy only when necessary.

Simple shell wrapper to proxy or tunnel or hop through intermediate
`ssh` servers, SOCKS proxies, or HTTP proxies (e.g. `corkscrew`), when
`ssh` would be unable to connect directly.

Most often useful on portable clients (e.g. laptops or tablets),
especially when only SOMETIMES behind a draconian firewall or outside
a network with limited `ssh` connections inbound.  In all cases, if you
have a VPN option, that will probably be better, but even in those
cases, this script should not interfere and allows you to be "lazy" in
your VPN usage.

Intended for use inside your `~/.ssh/config` file, like these examples:

Outbound http proxy (e.g., behind a restrictive outbound firewall):

    Host *
      ProxyCommand $HOME/.ssh/proxy -p httpproxy.foo.com:8080 %h %p

If not specified on the command line, the proxy will default to the
first non-empty evironment variables, in order:
* `http_proxy`
* `HTTP_PROXY`
* `https_proxy`
* `HTTPS_PROXY`

Inbound hop, when limited inbound ssh connections allowed:

    Host *
      ProxyCommand $HOME/.ssh/proxy -h ssh-hop.foo.com:8080 %h %p

In either case, the script will first attempt a direct connection,
avoiding any overhead.

Another feature enables you to setup "pseudo" hostnames in your
`~/.ssh/config` file and use a `sed` regexp to modify the destination
hostname before attempting a connection.  For example:

    Host prefix-*
      ProxyCommand $HOME/.ssh/proxy -S 's/^prefix-//' -h subnet-bastion %h %p

In those cases, the destiation hostname would have the `prefix-`
removed before attempting to connect to the destiation server.  This
can be helpful if an entire subnet of hosts is behind a special
bastion server, and you need or want rather just have hostname resolution
happen on the bastion.  Side-effect: your `~/.ssh/known_hosts` file
will be populated with keys that still **include** the `prefix-`.

In general, this script assumes `ssh`, `netcat` (`nc`) and `corkscrew`
(an http-proxy available from http://www.agroman.net/corkscrew).  As
an alternative to corkscrew, some people have repoted success using
`desproxy` (http://sourceforge.net/projects/desproxy/).

Netcat is used both to detect the availablility of the destination or
proxy hosts as well as for direct connections.  Both are assumed to be
resident in your `PATH`.  If not, you may specify a different
http-proxy or alternate location for netcat on the command line (in
your ssh config) OR, of course, "use the source, Luke"...

Comments, suggestions, questions welcome.
