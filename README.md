buildpack-irc
=============

Host an IRC server with heroku.

Requires ruppell's sockets

`heroku addons:add ruppells-sockets`

Set the build pack URL

`heroku config:set BUILDPACK_URL=https://github.com/j-mcnally/buildpack-irc`

Create and ircd.conf in your heroku app's bircd folder

```
# servername::server description::numeric. 
# change numeric to something unique on the network

M:my.host.name::Whoot::1

# accept connections on port 6667 (clients) and port 4400 (servers)

P::::6667

# standard Y-lines
# client class: ping freq 90, no autoconnect, no limit on connections, 80k sendQ
# oper class (same as client class)
# server class: ping freq 30, connect freq 300 secs, no limit on connections, 3M sendQ

Y:1:90:0:0:80000
Y:10:90:0:0:750000
Y:20:90:300:0:3000000

# standard I-line, accept everyone, max. 3 clones

I:*@*:3:*@*::1

# standard I-line for IPv6 connections: max 5 clones from a /48
I:"*@::/0":5/48:nomatch::1

# C-line for accepting services connection on same machine
# if you change these values, change them in bircserv.cfg as well.


# which server has services power
# and which nicks are reserved (cannot be set by local clients)

U:services.server.name:ChanServ,NickServ,MemoServ,OperServ:

# every server can be hub

H:*::*:

# syntax for O:lines (needed to become IRCop)
# fill in your mask, your password, your name
# type /oper name password, in the irc client, to use.
#
# to encrypt your password, run mkpasswd, type your password, 
# press enter, use the output string as shown in the example.

O:*:password:username::10

# syntax for S:lines
# S:<spoofed host>:<password>:<*.host.cc|a.b.c.*|CIDR>:<ident>
#
# example
# S:network.org:pass:1.2.3.4:*
#
# Oper Sethost: /sethost <new ident> <new hostname> 
#               /mode <nick> +h <new ident>@<new hostname> 
# User Sethost: /sethost <new hostname> <password> 
#               /mode <nick> +h <new hostname> <password> 
# (text taken from quakenet asuka)
# make sure sethost=1 and vhoststyle= non 0 in bircd.ini

```

Now deploy the app to heroku.

Note this is not to be used as a full blown IRC server. Its useful for app error / deploy / ci reporting.
It may also work for small team collaboration.

Ruppells-sockets only support a free 50mb/day plan right now so that may be a limiting factor.

The irc server runs on worker so make sure that you

`heroku ps:scale web=0; heroku ps:scale worker=1`

