This is intended to be run on businesscard Debian on a cheap
VPS from Prometeus.

## Install
Install dependencies

    apt-get install apache2 \
    libapache2-mod-gnutls ssl-cert

Then unpack the current directory into `/`.

## Configure SSL
Enable SSL if you want it.

    a2enmod ssl

Remove SSL from the configuration if you don't want it.

    # Command goes here.

## Apache sites
This creates an apache site that you can enable.

    a2ensite cgit

You should also disable other things that would interfere.

    ls /etc/apache2/sites-available

If you want to build from source, follow the directions
in the cgit readme. This will produce `cgit.cgi`, `cgit.css`
and `cgit.png`. Move these to the places that they are in
in the current repository.

Write a script that updates `/etc/cgitrc`.
