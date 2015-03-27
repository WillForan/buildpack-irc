buildpack-irc
=============

Host an IRC server with heroku. Forked from https://github.com/j-mcnally/buildpack-irc

*FAILING*: Does not work (2015-03-27)


```bash
# get
git clone git@github.com:j-mcnally/buildpack-irc.git
cd buildpack-irc
git submodule add https://bitbucket.org/ruppells/sockets-connect.git lib/sockets-connect

# configure
vim bircd/ircd.conf 

# app-ize
heroku create
heroku addons:add ruppells-sockets
# The irc server runs on worker 
heroku ps:scale web=0 && heroku ps:scale worker=1
heroku buildpack:set https://github.com/WillForan/buildpack-irc
git push heroku master
```



Note this is not to be used as a full blown IRC server. Its useful for app error / deploy / ci reporting.
It may also work for small team collaboration.

As of 2015-03-27, [Ruppells-sockets](https://devcenter.heroku.com/articles/ruppells-sockets) only support a free [50mb/day plan](https://addons.heroku.com/ruppells-sockets).



