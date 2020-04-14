---
created: 1280430942
layout: post
redirect_from:
- article/78/
- node/78/
- drush-and-drupal-development-setup-for-opensuse/
title: Drush, Drush Make, other Drupal packages, and development setup for openSUSE
---
I have recently added <a href="http://drupal.org/project/drush">drush</a> and <a href="http://drupal.org/project/drush_make">drush make</a> packages to <a href="http://download.opensuse.org/repositories/home:/boombatower/">my openSUSE repository</a>. For more information or to report bugs on the packages please visit their respective project pages: <a href="https://build.opensuse.org/package/show?package=drush&project=home:boombatower">drush</a> and <a href="https://build.opensuse.org/package/show?package=drush_make&project=home:boombatower">drush_make</a>.

To install the packages you can use the <a href="http://software.opensuse.org/search?q=drush">one-click installers</a> provided by the build service or manually add my repository and install the packages as shown bellow.

```bash
su
zypper ar http://download.opensuse.org/repositories/home:/boombatower/openSUSE_11.[2 or 3]/ home:boombatower
zypper in drush drush_make
```

Also note my existing <a href="http://drupal.org">Drupal</a> packages: <a href="https://build.opensuse.org/package/show?package=drupal-dev&project=home:boombatower">drupal-dev</a> and <a href="https://build.opensuse.org/package/show?package=drupal-vhosts&project=home:boombatower">drupal-vhosts</a>, as well as the <a href="http://software.opensuse.org/search?q=lamp+drupal">LAMP Drupal one-click pattern</a>. The latter package (drupal-vhosts) is very useful in setting up a multi-drupal version, multi-subdomain work environment.

To use simply install and run the command to point the virtual hosts to the directory containing your Drupal code.

```bash
su
zypper in drupal-vhosts
drupal-vhost /path/to/main/software/directory
```

Either edit the hosts file directory or use <em>YaST -> Network Services -> Hostnames</em> to add an entry for every Drupal version you wish to run (package currently supports 6, 7, and 8). The relevant lines from my /etc/hosts file are as follows.

```
127.0.0.1       d7x.loc 
127.0.0.1       d6x.loc 
```

For my setup I use <em>/home/boombatower/software</em> for all my code with Drupal cores in <em>drupal-7</em> and <em>drupal-6</em> directories respectively. If you want to have subdomains for your sites just add more entries to <em>/etc/hosts</em> and use the respective Drupal sites directories.

Personally, I then create symbolic links to all my modules so that the code resides in the root of the <em>software</em> directory, but can be used by any respective site. This makes the paths to modules and what not much shorter and easier to reference from multiple specific sub-sites and what not. For example to link pathauto to the all modules directory for Drupal 7 I would execute the following.

```bash
ln -s ~/software/pathauto ~/software/drupal-7/sites/all/modules
```

Or from within the <em>sites/all/modules</em> directory as I tend to do.

```bash
ln -s ~/software/pathauto .
```

Also note, to enable mod_rewrite and get clean URLs to work simply go to <em>YaST -> System -> /etc/sysconfig Editor</em> then <em>Network -> WWW -> Apache 2 -> APACHE_MODULES</em> and add <em>rewrite</em> to the end of the line. You can do so manually of course as well.

In order for the virtual host changes and apache module addition to take effect you will need to restart apache and for the /etc/hosts changes you need to restart the network which you can do with the following commands run as root.

```bash
rcapache2 restart
rcnetwork restart
```

The end result of all this work is beautiful URLs like: http://d7x.loc/node/1, http://foo.d7x.loc/user, and http://d6x.loc/.

I also create a similar structure within MySQL. First, I set an easy to remember MySQL root password since there really no reason for it not to be easy to remember and it is helpful when having to enter it a lot.

```bash
mysqladmin -u root password EASY_TO_REMEMBER_PASSWORD
```

Next setup a <em>drupal</em> user in MySQL and give the user all permissions to <em>d7x*</em> and <em>d6x*</em> named databases which allows us to use a single user for all our drupal sites (much easier to remember login info) without having to update privileges all the time. I name my databases the same as virtual hosts, so for <em>d7x.loc</em> I would have <em>d7x</em> as the database name and for <em>foo.d7x.loc</em> I would have <em>d7x-foo</em>.

```sql
CREATE USER 'drupal'@'localhost' IDENTIFIED BY  'EASY_TO_REMEMBER_PASSWORD';
GRANT USAGE ON * . * TO  'drupal'@'localhost' IDENTIFIED BY  'SAME_EASY_TO_REMEMBER_PASSWORD' ;

GRANT ALL PRIVILEGES ON  `d7x%` . * TO  'drupal'@'localhost';
GRANT ALL PRIVILEGES ON  `d6x%` . * TO  'drupal'@'localhost';
```

Anytime you want to add a database for a new site simply run the following.
```sql
CREATE DATABASE  `DATABASE_NAME` ;
```

Enjoy your fancy development environment!
