[Archived] Deploy Nextcloud with MariaDB in a Podman Pod
========================================================

_Notice:_ This repository has been archived. Future work is going to happen in the Ansible Collection [tronde.nextcloud](https://codeberg.org/Tronde/nextcloud) at [codeberg.org](https://codeberg.org).

With this ansible role you can deploy Nextcloud and MariaDB container in a podman-pod(1) to get a running Nextcloud instance ready to use from the local host. To reach this Nextcloud from a remote location you have two options:

 1. Use a reverse proxy like NGINX to forward requests to your Nextcloud pod.
 2. Listen on all interfaces for incoming traffic for the pod.

I strongly recommend option 1.

This role was tested on Fedora 35 and Debian 11 (Bullseye) so far. Please let me know if you run it with other Distributions and version, so I can add them to _meta/main.yml_. Any feedback is welcome.

Requirements
------------

* Collection containers.podman

To install this collection use: `ansible-galaxy collection install containers.podman`

Role Variables
--------------

All variables needed to deploy a pod containing containers for Nextcloud and
MariaDB are defined in _defaults/main.yml_ and set to example values. You
have to change these values or set them in _vars/main.yml_ to fit your needs.
Please keep your passwords as a secret. Use ansible-vault(1) to protect them.

### Variables in defaults/main.yml

```
# Podman volumes for Nextcloud
NC_HTML: nc_html
NC_APPS: nc_apps
NC_CONFIG: nc_config
NC_DATA: nc_data

# Podman volume for MariaDB
MYSQL_DATA: mysql_data

# MySQL/MariaDB vars
MYSQL_DATABASE: nextcloud
MYSQL_USER: nextcloud
MYSQL_PASSWORD: ToPSeCrEt2021!
MYSQL_ROOT_PASSWORD: ToPSeCrEt2021!
MYSQL_HOST: 127.0.0.1

# Vars for MariaDB container
MARIADB_SYSTEMD_PATH: ~/.config/systemd/user/
MARIADB_CONMON_PIDFILE: /tmp/mariadb_conmon.pid
MARIADB_IMAGE: docker.io/library/mariadb:10.5.7
MARIADB_NAME: nc_mariadb

# Nextcloud vars
NEXTCLOUD_ADMIN_USER: nc_admin
NEXTCLOUD_ADMIN_PASSWORD: VSnfD2021!

# SMTP vars
SMTP_HOST: smtp.example.com
SMTP_SECURE: tls # ssl to use SSL, or tls zu use STARTTLS
SMTP_PORT: 587 # (25, 465 for SSL, 587 for STARTTLS)
SMTP_AUTHTYPE: LOGIN
SMTP_NAME: bob@example.com
SMTP_PASSWORD: MailSecret1!
MAIL_FROM_ADDRESS: no-reply@example.com

# Vars for podman-pod(1)
POD_NAME: nc_pod
POD_INFRA_NAME: nc_pod_infra
POD_PORT: 80
POD_INFRA_CONMON_PIDFILE: /tmp/nc_pod_infra.pid
POD_SYSTEMD_PATH: ~/.config/systemd/user/

# Vars for Nextcloud container
NC_CONMON_PIDFILE: /tmp/nc_conmon.pid
NC_SYSTEMD_PATH: ~/.config/systemd/user/
NC_IMAGE: docker.io/library/nextcloud:22.2-apache
NC_NAME: nextcloud
```

With this default configuration podman will choose a random host port that is not in use to connect it with the port of the pod.

Example Playbook
----------------


    - hosts: servers
      remote_user: root
      roles:
         - ansible_role_deploy_nextcloud_with_mariadb_pod

License
-------

GPL version 2 or later

This role comes as it is, without any warranty. Use it at your own risk.

Author Information
------------------

Author: Joerg Kastning
URL: https://www.my-it-brain.de/wordpress/zu-meiner-person/
E-Mail: joerg(dot)kastning(aet)gmail(dot)com
