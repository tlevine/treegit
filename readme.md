This is intended to be run on businesscard Debian on a cheap
VPS from Prometeus.

## Install
Set up SSH keys if you want.

    scp-copy-id root@git.thomaslevine.com

Install dependencies

    apt-get install apache2

Then unpack the current directory into `/`.

    scp -r etc var home root@git.thomaslevine.com

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

## Use
Add a cgitrc inside each of your repositories, and push them
to /home/tlevine
