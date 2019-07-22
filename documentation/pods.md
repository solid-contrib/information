Pods are personal online data stores. One can either use a Pod provider who offers data storage as a service or self host a Pod. 

# Installing and Running Node Solid Server

The primary implementation offered of Solid is written in Javascript based on [Node.js](https://nodejs.org/). It should run on versions later than version 8. It is being [developed on Github](https://github.com/solid/node-solid-server) and is released with the liberal MIT license to [NPM](https://www.npmjs.com/package/solid-server). To give it a spin, you can download Node.js, including npm, and get it running quickly. There is an extensive [module README](https://github.com/solid/node-solid-server#install that provides an overview of the many options of the server and is very useful a development environment. For a simple single-user production installation, there is an example for Debian systems:

## Example install: Solid Server on Debian GNU/Linux


Following [these instructions](https://nodejs.org/en/download/package-manager/#debian-and-ubuntu-based-linux-distributions), we install Node.js and npm like this

$ curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
$ sudo apt-get install -y nodejs build-essential
As root, you can now install the server globally:

$ npm install -g solid-server
Now, you need to decide what is going to be your hostname, we will use your.host.example.org in the following. You should also decide where you want to keep the files the server needs to run. The configuration, data, and supporting metadata database can be kept separate if you want, but we’ll keep them together.


Since everything going on with a Solid server is encrypted, you will need a SSL key and certificate. This can be obtained from e.g. [Let’s Encrypt](https://letsencrypt.org/). The easiest way to do this is to use certbot, to do this on Debian Stable, you need a backported package. To obtain that, add this line to your /etc/apt/sources.list:

$ deb http://ftp.debian.org/debian stretch-backports main
Then, certbot can be installed with:

$ apt-get update
$ apt-get -t stretch-backports install certbot
Then, request and install the certificate using:

$ certbot certonly --authenticator standalone -d your.host.example.org
Then, go,

$ solid init
You will be prompted for several things, it could look like this:

? Path to the folder you want to serve. Default is /var/www/your.host.example.org/data
? SSL port to run on. Default is 8443 443
? Solid server uri (with protocol, hostname and port) https://your.host.example.org
? Enable WebID authentication Yes
? Serve Solid on URL path /
? Path to the config directory (for example: /etc/solid-server) /var/www/your.host.example.org/config
? Path to the config file (for example: ./config.json) /var/www/your.host.example.org/config.json
? Path to the server metadata db directory (for users/apps etc) /var/www/your.host.example.org/.db
? Path to the SSL private key in PEM format /etc/letsencrypt/live/your.host.example.org/privkey.pem
? Path to the SSL certificate key in PEM format /etc/letsencrypt/live/your.host.example.org/fullchain.pem
? Enable multi-user mode No
? Do you want to have a CORS proxy endpoint? Yes
? Serve the CORS proxy on this path /proxy
? Do you want to set up an email service? No
config created on /root/config.json
Then, you need to create the paths that you entered. You would also need to copy the config.json file to where you indicated it should be.


You should be able to start the server now with:

$ solid start
However, we recommend that you use e.g. systemd to automatically start and stop the server.
Since it is a security risk to run the server as root, you should create a user for it, with e.g.

$ adduser --system --ingroup www-data --no-create-home solid
Then, create a file /lib/systemd/system/solid.service containing

[Unit]
Description=solid - Social Linked Data
Documentation=https://solid.inrupt.com/docs/
After=network.target

[Service]
Type=simple
User=solid
WorkingDirectory=/var/www/your.host.example.org
ExecStart=/usr/bin/solid start -v
Restart=on-failure

[Install]
WantedBy=multi-user.target
Then, symlink that:

$ ln -s /lib/systemd/system/solid.service /etc/systemd/system/multi-user.target.wants/
Now, make the directories owned by the user:

$ cd /var/www/your.host.example.org/
$ chown solid:www-data config/ data/ .db/
You may also need to make the config.json file readable to the unprivileged user.


Now, starting the server should be done by:

$ systemctl start solid.service
You should now also have the other facilities provided by systemd available, and it should start automatically after the machine is booted.


The certificate needs to be renewed every few months, and you should modify your Let’s Encrypt setup to use the webroot plugin, which is better to use when the server is running. To do so, modify the file /etc/letsencrypt/renewal/your.host.example.org.conf . Make sure the authenticator line reads:

authenticator = webroot
Also set:

webroot_path = /var/www/your.host.example.org/data
[[webroot_map]]
your.host.example.org = /var/www/your.host.example.org/data
Then, create a cron job by creating a file in /etc/cron.daily/ that contains:

#!/bin/bash
certbot renew -w /var/www/your.host.example.org/data/
Now, you should have a working Solid server! Enjoy!

# Self Hosting Pods

* https://github.com/shingyeung/contrib/

* [Solid server with docker on a Synology NAS](https://www.bourgeoa.ga/solid-wiki/index.php?title=Solid_server_with_docker_on_a_Synology_Nas) with use of freenom and Lets'encypt certificate with sslforfree

* Docker image [aveltens/solid-server](https://hub.docker.com/r/aveltens/solid-server) with [deployment examples](https://github.com/angelo-v/docker-solid-server/tree/master/examples).
