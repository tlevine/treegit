This is intended to be run on businesscard Debian on a cheap
VPS from Prometeus.

Install dependencies

    apt-get install apache2 \
    libapache2-mod-gnutls ssl-cert

Then unpack the current directory into `/`.
This creates an apache site that you can enable.

    a2enmod ssl
    a2ensite cgit

You should also disable other things that would interfere.

If you want to build from source, follow the directions
in the cgit readme. This will produce `cgit.cgi`, `cgit.css`
and `cgit.png`. Move these to the places that they are in
in the current repository.

Write a script that updates `/etc/cgitrc`.
