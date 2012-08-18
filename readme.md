treegit
==================
**treegit** is an off-the-self cgit server with some helpers. It is intended to
be run on Debian 5.0 on a cheap VPS dedicated to treegit, but it could work in
other ways with some adjustment.

## Install
Commands in this "Install" section get run on a computer other than the server
we're configuring. First, choose your domain name.

    export TREEGIT_DOMAIN_NAME=git.thomaslevine.com

You'll have to run those lines again if you open a new shell.

Set up SSH keys if you want.

    scp-copy-id root@$TREEGIT_DOMAIN_NAME

Install dependencies

    ssh root@$TREEGIT_DOMAIN_NAME 'apt-get install apache2'

Unpack the current directory into `/`.

    scp -r etc usr var root@$TREEGIT_DOMAIN_NAME

### Building from source
If you want to build from source, follow the directions
in the cgit readme. This will produce `cgit.cgi`, `cgit.css`
and `cgit.png`. Move these to the places that they are in
in the current repository, then unpack the repository to `/`.

## Configure
SSH to the server

    ssh root@$TREEGIT_DOMAIN_NAME 

The rest of this "Configure" section will run on the server, inside of that SSH
session. First, set username, domain name and email address you would like to
use. The user does not have to have been created.

    export TREEGIT_USERNAME=tlevine
    export TREEGIT_DOMAIN_NAME=git.thomaslevine.com
    export TREEGIT_EMAIL_ADDRESS=tom@example.com
    treegit-configure

Disable any Apache sites that are enabled.

    rm /etc/apache2/sites-enabled/*

Then enable the cgit site. You get to choose between an SSL version and a
non-SSL version.

### Without SSL
This is all you need to run.

    a2ensite cgit

### With SSL
Enable SSL.

    apt-get install libapache2-mod-gnutls ssl-cert
    a2enmod ssl

Then enable cgit.

    a2ensite cgit-ssl

### Test
Now log out of the SSH session and test that it's working by running the tests.

    ./tests

## Use
Once you've installed it, follow the directions that appear here.

    echo http://$TREEGIGT_DOMAIN_NAME/?p=about

## To do

* Switch Apache for Nginx
* Set up basic http authentication for the SSL site.
