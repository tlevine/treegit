treegit
==================
**treegit** is an off-the-self cgit server with some helpers. It is intended to
be run on Debian 5.0 on a cheap VPS dedicated to treegit, but it could work in
other ways with some adjustment.

## Install/Update
Commands in this "Install" section get run on a computer other than the server
we're configuring. It's fine to run these commands on an existing installation,
(Configuration files will simply be replaced.) so that's a reasonable way to
update treegit.

First, choose your domain name.

    export TREEGIT_DOMAIN_NAME=git.thomaslevine.com

You'll have to run those lines again if you open a new shell.

Set up SSH keys if you want.

    scp-copy-id root@$TREEGIT_DOMAIN_NAME

Install dependencies

    ssh root@$TREEGIT_DOMAIN_NAME 'apt-get update && apt-get upgrade && apt-get install apache2 markdown'

Unpack the current directory into `/`.

    scp -r etc usr var root@$TREEGIT_DOMAIN_NAME:/

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

Create the user

    useradd $TREEGIT_USERNAME
    mkdir -p /home/$TREEGIT_USERNAME/.ssh
    cp ~/.ssh/authorized_keys /home/$TREEGIT_USERNAME/.ssh # optional
    chown -R $TREEGIT_USERNAME: /home/$TREEGIT_USERNAME

Then enable the cgit site. You get to choose between an SSL version and a
non-SSL version.

### Without SSL
This is all you need to run.

    a2ensite cgit
    service apache2 reload

### With SSL
Enable SSL.

    apt-get install libapache2-mod-gnutls ssl-cert
    a2enmod ssl

Then enable cgit.

    a2ensite cgit-ssl

This site is set up to perform identification of both the server and the client
over SSL. It expects a self-signed certificate stored in `/etc/ssl/certs/treegit.crt`,
with its key file in `/etc/ssl/private/treegit.key`. Furthermore, it expects
that the same certificate be used to sign the client key, which must be imported
in the web browser. Here is how you can generate all these files.

    # Generate
    openssl genrsa -des3 -out treegit.key 4096

    # Request
    openssl req -new -key treegit.key -out treegit.csr

    # Remove passphrase
    cp treegit.key treegit.key.org
    openssl rsa -in treegit.key.org -out treegit.key

    # Sign
    openssl x509 -req -days 2000 -in treegit.csr -signkey treegit.key -out treegit.crt

    # Convert to pkcs12 for client authentication.
    openssl pkcs12 -export -in treegit.crt -inkey treegit.key -out treegit.p12 -name TreeGit

First, upload the files for server identification.

    # Upload
    scp treegit.key root@$TREEGIT_DOMAIN_NAME:/etc/ssl/private/treegit.key
    scp treegit.crt root@$TREEGIT_DOMAIN_NAME:/etc/ssl/certs/treegit.crt

Second, import the `treegit.crt` file in your browser to identify the server to
your browser.

Third, import the `treegit.p12` file in your browser to identify your browser
to the server.

Finally, reload the apache configuration.

    service apache2 reload

## Use
The system should be ready for use. You can log out of the SSH and follow the
directions on the about page.

    echo http://$TREEGIT_DOMAIN_NAME/?p=about

You can test that it's working by running the tests.

    export TREEGIT_USERNAME=tlevine
    export TREEGIT_DOMAIN_NAME=git.thomaslevine.com
    ./tests


## To do

* Set up hooks so that the cgitrc and description files can be versioned inside the
    repository and automatically placed in the git directory on the server.
* Switch Apache for Nginx
* Make it easier to back up configuration of different sites but still apply updates.
