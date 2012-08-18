This is intended to be run on businesscard Debian on a cheap
VPS from Prometeus.

## Install
Set up SSH keys if you want.

    scp-copy-id root@git.thomaslevine.com

Install dependencies

    apt-get install apache2

Then unpack the current directory into `/`.

    scp -r etc home usr var root@git.thomaslevine.com

### Building from source
If you want to build from source, follow the directions
in the cgit readme. This will produce `cgit.cgi`, `cgit.css`
and `cgit.png`. Move these to the places that they are in
in the current repository, then unpack the repository to `/`.

## Configure
Disable any Apache sites that are enabled.

     /etc/apache2/sites-enabled/*

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
Test that it's working by running the tests.

    ./tests

## Use
Follow the directions that appear in http://git.thomaslevine.com/?p=about

## To do

* Switch Apache for Nginx
* Parametrize the user (not just tlevine) and domain (not just git.thomaslevine.com)
* Set up basic http authentication for the SSL site.
