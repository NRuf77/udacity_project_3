# Udacity course project: web server configuration
Submitted by Nikolaus Ruf as part of the requirements for the full stack web
developer course.


## Web server address
The item catalog application developed for the course is hosted on a Ubuntu web
server from AWS Lightsail with static public IP 35.161.178.17.

Login credentials for the Udacity grader will be supplied separately with the
project submission.

To test the application website, please go to

http://35.161.178.17.xip.io

If you use the naked IP to visit the website, Google OAuth2 will not work as it
requires a URL with a proper domain name.

The service provided free of charge by xip.io turns the static IP into a proper
URL without the need to register a domain.


## Web server configuration
The web server has been configured as per the course specification:
- Users can only log in using key-based authentication and remote login for
  root has been disabled completely.
- The SSH port has been changed to a non-standard port.
- The UFW firewall has been configured to accept requests only for the ports
  specified in the project requirements.


## Steps performed during app installation

### Linux programs
The following additional programs have been installed using apt-get:
- autoremove
- finger
- apache2
- libapache2-mod-wsgi
- python-pip

Note: git was already installed on the Ubuntu server; a PostgreSQL database has
not been installed as the item catalog was built using sqlite3. 


### Python packages
The following Python packages have been installed using pip install:
- SQLAlchemy
- bleach
- flask
- oauth2client
- requests


### Application code
- The git repository

  https://github.com/NRuf77/fullstack-nanodegree-vm

  containing the item catalog application has been cloned to

  > /home/catalog_admin/catalog_git/

  on the server.

- The actual application code in 

  > /home/catalog_admin/catalog_git/vagrant/catalog/

  has been copied to the web application directory

  > /var/www/html/

  The owner and group of all application files has been changed to the Apache
  web server user "www-data".
  File permission have been changed to 750 for directories and 640 for
  application files.


### Application setup
- The client secret for Google OAuth2 services which is not included with the git
  repository has been copied to

  > /var/www/html/data/client_secret.json

- A test database (sqlite3) has been created and pre-filled in

  > /var/www/html/data/catalog.db

  using the scripts

  > /var/www/html/scripts/db_setup.py

  > /var/www/html/scripts/db_fill.py

- The Google OAuth2 authentication service has been extended to accept

  http://35.161.178.17.xip.io

  both as javascript origin and as redirect URI (via the Google developer
  site).

- The Apache web server configuration file

  > /etc/apache2/sites-enabled/000-default.conf

  has been extended by the following lines of code:

  > WSGIDaemonProcess catalog_app home=/var/www/html

  > WSGIScriptAlias / /var/www/html/start_app.wsgi process-group=catalog_app

  These ensure that the working directory for the app is /var/www/html and that
  the server starts serving the app from start_app.wsgi.
