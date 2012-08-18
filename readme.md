treegit
==================
**treegit** is an off-the-self cgit server with some helpers. It is intended to
be run on Debian 5.0 on a cheap VPS dedicated to treegit, but it could work in
other ways with some adjustment.

## Install
Unless I say otherwise, run all this stuff on a computer other than the server
we're configuring. First, choose your username and domain name. (We only need
these for the installation, not for use afterwards.)

    export TREEGIT_USERNAME=tlevine
    export TREEGIT_DOMAIN_NAME=git.thomaslevine.com

You'll have to run those lines again if you open a new shell.

Set up SSH keys if you want.

    scp-copy-id root@$TREEGIT_DOMAIN_NAME

Install dependencies

    ssh root@$TREEGIT_DOMAIN_NAME 'apt-get install apache2'

Unpack the current directory into `/`.

    scp -r etc home usr var root@$TREEGIT_DOMAIN_NAME

### Building from source
If you want to build from source, follow the directions
in the cgit readme. This will produce `cgit.cgi`, `cgit.css`
and `cgit.png`. Move these to the places that they are in
in the current repository, then unpack the repository to `/`.

## Configure
Disable any Apache sites that are enabled.

    ssh root@$TREEGIT_DOMAIN_NAME 'rm /etc/apache2/sites-enabled/*'

Then enable the cgit site. You get to choose between an SSL version and a
non-SSL version.

### Without SSL
This is all you need to run.

    ssh root@$TREEGIT_DOMAIN_NAME 'a2ensite cgit'

### With SSL
Enable SSL.

    ssh root@$TREEGIT_DOMAIN_NAME 'apt-get install libapache2-mod-gnutls ssl-cert'
    ssh root@$TREEGIT_DOMAIN_NAME 'a2enmod ssl'

Then enable cgit.

    ssh root@$TREEGIT_DOMAIN_NAME 'a2ensite cgit-ssl'

### Test
Test that it's working by running the tests.

    ./tests

## Use
Once you've installed it, follow the directions that appear here.

    echo http://$TREEGIGT_DOMAIN_NAME/?p=about

## To do

* Switch Apache for Nginx
* Parametrize the user (not just tlevine) and domain (not just git.thomaslevine.com)
* Set up basic http authentication for the SSL site.
