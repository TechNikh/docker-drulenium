Drulenium testing with Docker
==============================

Quick and easy to use Docker container for your *local Drulenium testing*. It contains a LAMP stack and an SSH server, along with an up to date version of Drush. It is based on [Debian Jessie](https://wiki.debian.org/DebianJessie).

Installation
------------
### Using graphical interface through Kitematic
http://drulenium.org/how-to-install-drulenium-locally-using-docker-gui
### Github

Clone the repository locally and build it:

	git clone git@github.com:TechNikh/docker-drulenium.git
	cd docker-drulenium
	docker-compose up

Notice that there are several branches. The `master` branch always refers to the current recommended major Drulenium version.

Running it
----------
1. After ```docker-compose up``` Go to http://[my-docer-machine-ip-address]:8080/
2. You can get the IP by running the command ```docker-machine ip```
3. Log into the Drupal site as admin with username & password -> `admin:admin`
4. Configure Drulenium module settings at http://[my-docer-machine-ip-address]:8080/drulenium/settings/local with selenium Host URL like http://[my-docer-machine-ip-address]:4444/wd/hub
5. Choose the local server as Snapshot server at http://[my-docer-machine-ip-address]:8080/drulenium/settings
6. And run your tests at http://[my-docer-machine-ip-address]:8080/drulenium/vr

Summary
-------

This image contains:

* Apache 2.4
* MySQL 5.5
* PHP 5.6
* Drush 8
* Drupal 7
* Composer
* PHPMyAdmin

When launching, the container will contain a fully-installed, ready to use Drupal site.

### Passwords

* Drupal: `admin:admin`
* MySQL: `root:` (no password)
* SSH: `root:root`

### Exposed ports

* 80 (Apache)
* 22 (SSH)
* 3306 (MySQL)

### Using Drush

Using Drush aliases, you can directly execute Drush commands locally and have them be executed inside the container. Create a new aliases file in your home directory and add the following:

	# ~/.drush/docker.aliases.drushrc.php
	<?php
	$aliases['docker-drulenium'] = array(
	  'root' => '/var/www',
	  'remote-user' => 'root',
	  'remote-host' => 'localhost',
	  'ssh-options' => '-p 8022', // Or any other port you specify when running the container
	);

Next, if you do not wish to type the root password everytime you run a Drush command, copy the content of your local SSH public key (usually `~/.ssh/id_rsa.pub`; read [here](https://help.github.com/articles/generating-ssh-keys/) on how to generate one if you don't have it). SSH into the running container:

	# If you forwarded another port than 8022, change accordingly.
	# Password is "root".
	ssh root@localhost -p 8022

Once you're logged in, add the contents of your `id_rsa.pub` file to `/root/.ssh/authorized_keys`. Exit.

You should now be able to call:

	drush @docker.docker-drulenium cc all

This will clear the cache of your Drupal site. All other commands will function as well.

### MySQL and PHPMyAdmin

PHPMyAdmin is available at `/phpmyadmin`. The MySQL port `3306` is exposed. The root account for MySQL is `root` (no password).
