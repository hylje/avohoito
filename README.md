
This is a constantly updated documentation for OKF Finland's main server stuff.

Language of choice is English, for fast documentation Finnish is OK if the other option would be no documentation at all.

Table of contents

[[TOC]]

* * *


# 0. Basic Info
* Sysadmin page: [http://okf.fi/sysadmin/](http://fi.okfn.org/sysadmin/)
* Contact email: [sysadmin@okf.fi](mailto:sysadmin@okf.fi)
* Slack [https://okffi.slack.com/messages/sysadmin](https://okffi.slack.com/messages/sysadmin)
* FB-chat for sysadmins <no link - ask Tarmo to add> (is being phased out)
* User rights request form [http://bit.ly/okffi_user_rights](http://bit.ly/okffi_user_rights)
* Old documentation in Gdocs [http://okf.fi/server](http://okf.fi/server)

## Main Production Server AVOHOITO
avohoito.okf.fi
159.255.193.67

Responsible main admins: Tarmo Toikkanen and Jaakko Korhonen (also SUDO rights given to many people for setting up various services and sites)

ubuntu 12.04
1 CPU
4G RAM on VMware, scalable. 13.8.2014 added 1G for troubleshooting reasons. Later +1G, now at 6GB.
1 Gb peak bandwidth with 100 Mb ensured.
Traffic quota outwards 500 GB per month, inwards no limit.
Off-site backed up storage 100G, 9€/month, daily snapshot, weekly full replication.
Management, updates etc sysadmin support by Saima.

## Dedicated Production Server TIETOPYYNTO
[http://tietopyynto.fi](http://tietopyynto.fi)

159.255.193.72
Responsible main admins: 
ubuntu 12.04
1 CPU
3G RAM on VMware, scalable. 13.8.2014 added 1G for troubleshooting reasons.
1 Gb peak bandwidth with 100 Mb ensured.
Traffic quota outwards 500 GB per month, inwards no limit.
Off-site backed up storage 100G, 9€/month, daily snapshot, weekly full replication.

## Dedicated Production Server AVOKONTTI

ProServer PlusPower X6 2.0
32 GB DDR3-RAM
AMD Opteron 3280
Unlimited Bandwidth
Software-Raid 1
2 x 2 TB SATA II-HDD, 7.200 rpm

## Admin Tool Info

dns zones [https://pwadmin.saimanet.net/](https://pwadmin.saimanet.net/) 
postfix alias [https://admmail.saimanet.net/](https://admmail.saimanet.net/) 

**SMTP**
smtp.saimanet.net

## Test Server Info

No development or test servers as of yet. Contact [sysadmin@okf.fi](mailto:sysadmin@okf.fi) for applications. 
500M RAM 13€/month. Some can be included in existing budget.
Do not use Heroku, Digital Ocean or any other service if you need your servers to be managed by OKF sysadmins. We are in a phase where we consolidate the scarce resources and efforts to this one environment.

# 1. Domain names

Finnish domain names and handled by OKFFI’s own account at domain.fi. DNS is handled by Saimanet.

The following domains are in our DNS management.
* Directed mainly to avohoito:
* avoindemokratia.fi, .net, .com
* avointieto.fi
* datademo.fi
* datakoulu.fi
* hack4.fi
* okf.fi
* okfn.fi
* Directed to tietopyynto:
* tietopyynto.fi
* tietopyynto.okf.fi
* Directed to ckan:
* ckan.okf.fi
* Not directed anywhere:
* omadata.fi

Some services are merely routed through our server and go to another service:

* **datakoulu.fi: **[http://fi.okfn.org/datakoulu/](http://fi.okfn.org/datakoulu/)
* **avoindemokratia.fi: **[http://fi.okfn.org/avoin-demokratia-esimerkit/](http://fi.okfn.org/avoin-demokratia-esimerkit/) **(**avoindemokratia.com, avoindemokratia.net)
* **okf.fi:** uses the lessn service for shortlinks. In case of an unidentified shortlink, merely redirects to [http://fi.okfn.org/](http://fi.okfn.org/) which is our main web site, hosted by OKFN.

Acquisition of new domains: contact [sysadmin@okf.fi](mailto:sysadmin@okf.fi) with the request, listing the domains you need, and which project will pay for the costs. Finnish .fi domains cost 50€ for 5 years, international domains cost $15/year (prices may change).

# Instructions for adding new sercvices

Register to Slack in https://okffi.slack.com/messages/sysadmin/, or email the following info:
 - Service name
 - Are you putting a service up for debug or production in this phase. avohoito.okf.fi is the production server, for development purposes temporary servers are provisioned.
 - OKFFI responsible project or working group.
 - Responsible technical person
 - Source code repository
 - Memory and data storage requirement expectations
 - Server applications required
 - Service address, SOMETHING.okf.fi, or own domain. OKFFI can also register domains, ask through slack or sysadmin(a)okf.fi
 - Repository for service configuration files. We back up also service configurations to Github. There is a private repo available, if needed.
 - Ports reserved
 - Service url for monitoring

Order a shell account http://okf.fi/sysadmin

Install the services. For development purposes temporary servers are provisioned.

Configure Nagios
 - Add your shell account name to /etc/nagios3/htpasswd.conf, with different password.
 - Add your account name to contacts.cfg
 - Add the configuration file for your service, in /etc/nagios3/conf.d/okffi/, you can use the existing ones as template.

Inform progress and issues through Slack or sysadmin(a)okf.fi

# Installed Services

<em>This page is created with the <a href="http://okfnlabs.org/listify/" target="_blank">Listify -tool by the OKFN Labs</a> and the same data can bee seen at <a href="https://docs.google.com/spreadsheet/ccc?key=0Au5B9VrRkisPdFBGMlVhamJ1ZUlvMGhsTURYRjhUOEE&amp;usp=sharing" target="_blank">this Google Spreadsheet</a>.</em>

<iframe src="http://okfnlabs.org/listify/embed.html?url=https%3A%2F%2Fdocs.google.com%2Fspreadsheet%2Fccc%3Fkey%3D0Au5B9VrRkisPdFBGMlVhamJ1ZUlvMGhsTURYRjhUOEE%26usp%3Dsharing" width="800" height="5000" frameborder="0"></iframe>

# 2.a. Installed Services and Applications (AVOHOITO)

For each service there will be service-specific system account and someone responsible for that account. System accounts will not have passwords but will be accessed through sudo.

Components are brought to production by installing from public sources with public installation instructions. If any of these do not exist, service-specific Github repos are build by default before transferring services to production. It is advisable to develop and test new services with configuration management and batch deployment in mind.

## Basic services (installed)

### Firewall

iptables

firewall with ufw

### Acct

acct accounting is installed as of 8th August 2014. It logs all user logins, issued commands, and resource usage. In case of tracking down who caused a problem, this can be useful. Some key commands to try (as root):

* ac:           summarize login accounting
* last:         show the people who have logged in, and when they’ve logged out
* lastcomm:     show which commands have been used
* sa:           summarizes process accounting

NB. acct is not sufficient to prevent malicious use of granted shell privileges. It’s merely helpful in seeing who may have accidentally caused a problem in another service, while working on another one.

### IRC

Instructions on using screen and irssi together: [http://quadpoint.org/articles/irssi/](http://quadpoint.org/articles/irssi/)

Irssi, Screen
Working IRC-nodes:
open.ircnet.org
irc.freenode.net
IRCin käyttö:
1.  ssh [KÄYTTÄJÄ]@okf.fi
2.  screen -S irssi irssi
3. Irssissä sano:
/SERVER ADD -auto -network IRCnet open.ircnet.net
/SERVER ADD -auto -network freenode irc.freenode.net
/CHANNEL ADD -auto #OKFFI freenode
/CHANNEL ADD -auto #avoindata freenode
/CHANNEL ADD -auto #OKFN freenode
5. irtautuminen screenistä: ctrl-a + ctrl-d (lyhyemmin sanoen: ^a^d)
6. Seuraavalla kerralla paluu screeniin: screen -r (tai screen -x)

Myös Slackiin pääsee IRCin kautta: https://okffi.slack.com/account/gateways

### Cron
Ajastetut komennot ovat kaikkien käytettävissä. "man crontab" antaa kattavat ohjeet.
Mutta oleellisesti: ajamalla komennon "crontab -e" voit muokata omaa ajastettujen komentojen listaasi. Uusi rivi, jolla olisi sisältö:
`15 3 * * * cd /var/www/okf/data/FOO; git pull`
ajaisi varttia yli kolme aamuyöllä FOO-kansiossa git pull-komennon, eli päivittäisi ko. kansioon aiemmin saman käyttäjän tekemän git-kloonin.

### Web server: nginx
nginx is running on port 80 and is the main web server (since 2014-10-07).
Folders served by nginx are mainly in /var/www.
Nginx can server static content efficiently. It can also be used to serve php/perl/python powered content using fastcgi (such as php5-fpm) or special services using proxying. See examples in /etc/nginx/sites-available/.
Run nginx -t after modifying configurations, so the syntax is checked. **If nginx shuts down, all our web services are down!**

### Web server: apache2 (being obsoleted!)

Apache2 can still be used to run virtual web sites, if the site requires some apache2-specific functionality.

Folders served by apache2 are mainly in /var/www.

New virtual hosts are set up in folder /etc/apache2/sites-available, by copying an existing configuration and modifying appropriately. A new site called "NAME.conf" is taken into use like this::

1. Add the new domain name to /etc/nginx/sites-available/apache-proxy (onto the line that says "server_name", so that nginx routes requests to apache.

2. `a2ensite NAME`

1. Creates a symlink to sites-enabled

3. `apachectl -t`

2. Checks that the configuration syntax is correct. **Remember to do this, since an invalid configuration file can shutdown the entire web server!**

4. `service apache2 reload`

3. Load the new configuration.

If something goes wrong, do something like "a2dissite NAME" and “service apache2 restart” to get back to the preceding situation.

### Blogipalvelu, Wordpress network/multisite: http://blog.okf.fi

Wordpress multisite on asennettu kansioon /var/www/okf/blog. Asennus näkyy verkossa osoitteessa [http://blog.okf.fi](http://blog.okf.fi). Pyynnöstä saa itselleen blogin ylläpidettäväksi. Sille voidaan pyynnöstä antaa jokin OKF:n hallinnoima domaini tai alidomaini, tai siihen voi ohjata itse omistamansa domainin. Teemojen ja pluginien asentaminen tapahtuu superadminin toimesta, eli pyyntöjä ylläpidolle jos on tarvetta lisäteemoille tai plugineille.

### Nagios monitoring

Running at address [http://status.okf.fi](http://status.okf.fi)

You can see the status with account "vieras". Ask the password from OKFFI core.

To gain more access, add a username/password combination for yourself. Use the same 

username you have for shell access, and also use that as the contact name in Nagios configurations.

htpasswd /etc/nagios3/htpasswd.users USERNAME

To add or modify monitored services, add and edit service definitions in

/etc/nagios3/conf.d/okffi/

and run

service nagios3 reload

To get notifications of service outages, add yourself to the contacts configuration and add you as the manager for appropriate services.

## Services for the core team

### Membership register

The membership register must be kept private, and only accessible to the chairman and the secretary of the association. The register is hosted as a git repository in ssh://okf.fi/var/okfcore/okffi-jasenrekisteri and access to it is only given to those people who need it.

Membership applications are handled online, at [http://fi.okfn.org/membership/](http://fi.okfn.org/membership/). The form results are handled by the secretary and presented at each board meeting for approval. After approval, the new members are added to various systems, as described in the documentation included in the membership register.

## Applications (installed)

### URL shortener: Lessn More

URL shorterner. Access it at [http://okf.fi/-](http://okf.fi/-)

User name and password are given to people who need to create okf.fi short urls.

### Open ERP

[http://erp.okf.fi](http://erp.okf.fi)

### Data web server - http://data.okf.fi

/var/www/okf/data on kansio, johon kaikki käyttäjät voivat lisätä julkaistavia tiedostoja. Kansion sisältö näkyy osoitteessa http://data.okf.fi

### Paatokset.fi

[http://jkl.paatokset.fi/](http://jkl.paatokset.fi/)

### GIS Geocode API

Responsible admin: Mikael Rekola

Geographical Information System Geocode API

http://api.okf.fi/gis/1/autocomplete.json

http://api.okf.fi/gis/1/geocode.json

* Maanmittauslaitoksen maastotietokannan teiden geometriat ja attribuutit

* Itellan osoitteet, postinumerot, postitoimipaikat ja kunnat

* Pistemuotoiset osoitteet

[http://api.okf.fi/console/](http://data.okf.fi/console/)

[http://api.okf.fi/gis/1/geocode.json?address=Siltasaarenkatu+15](http://data.okf.fi/gis/1/geocode.json?address=Siltasaarenkatu+15) 

[http://api.okf.fi/gis/1/geocode.json?lat=60.187058&lng=24.961563](http://data.okf.fi/gis/1/geocode.json?lat=60.187058&lng=24.961563) 

Technically, the service is done using perl, and is located in /var/www/data/gis/

nginx proxies to apache, which uses mod_perl to call the service.

### Rahankeräysrekisteri

1. asennettu Apache CouchDB 1.5.0 ja supervisor + tarvittavat lisäpaketit
2. asennettu rahankeräysrekisteri-sovellus hakemistoon /home/warmaster/WhipAroundRegistryEnvironment
3. supervisor asetettu käynnistämään WarApp-sovelluksesta 2 instanssia 127.0.0.1 porteissa 8880 ja 8881 nobody-käyttäjänä
4. supervisor kuuntelee http://127.0.0.1:9999 ja tunnukset löytyy conffista. Tuolta voi hallita WarApp-sovellusta ja seurata lokia.
5. muokattu Apache2 lataamaan proxy_balancer moduuli
6. lisätty Apache2 virtual_host konfiguraatio domaineille rahankeraysrekisteri.fi ja rahankeräysrekisteri.fi

### CKAN / Datakatalogi with datastore

All documentation, discussion and issues in [https://github.com/okffi/katalogi](https://github.com/okffi/katalogi)
[http://ckan.okf.fi](http://ckan.okf.fi)
Running as a docker container. See instructions at [http://docs.ckan.org/en/latest/maintaining/installing/install-using-docker.html](http://docs.ckan.org/en/latest/maintaining/installing/install-using-docker.html)

The containers have been created as follows:

$ docker run -d --name db ckan/postgresql

$ docker run -d --name solr ckan/solr

$ docker run -d -p 8081:80 --link db:db --link solr:solr --name ckan ckan/ckan

Nginx is configured to proxy the port 8081 and serve that as virtual host ckan.okf.fi.

If the containers are not running, they can be started with

$ docker start solr db ckan

* * *


# 2.b. Installed services and application (TIETOPYYNTO)

### Froide [http://tietopyynto.fi](http://tietopyynto.fi)

All documentation, discussion and issues in [https://github.com/okffi/tietopyynto/](https://github.com/okffi/tietopyynto/)


