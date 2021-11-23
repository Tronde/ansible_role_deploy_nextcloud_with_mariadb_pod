Deploy Nextcloud with MariaDB in a Podman Pod
=============================================

Deploy Nextcloud and MariaDB container as podman-pod(1).

**Warning:** This role is still under development and not ready to use in
production, yet. But comments and feedback on this role are much appreciated.

This role was tested on Fedora 35 and Debian 11 (Bullseye) so far. Please let
me know if you run it with other Distributions and version, so I can add them
to _meta/main.yml_.

Requirements
------------

* Collection containers.podman

Role Variables
--------------

All variables needed to deploy a pod containing containers for Nextcloud and
MariaDB are defined in _defaults/main.yml_ and filled with example values. You
have to change this values or set them in _vars/main.yml_ to fit your needs.
Please keep your passwords secret. Use ansible-vault(1) to protect them.

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
MYSQL_HOST: localhost

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

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      remote_user: root
      roles:
         - ansible_role_deploy_nextcloud_with_mariadb_pod

License
-------

GPL version 2 or later

Author Information
------------------

Author: Joerg Kastning
URL: https://www.my-it-brain.de/wordpress/zu-meiner-person/
E-Mail: joerg(dot)kastning(aet)gmail(dot)com
