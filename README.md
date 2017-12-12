dachinat.nextcloud fork
=======================

This fork uses `10.2.10-MariaDB` stable

* `nextcloud_ssl_autogen: false`  
Support not to generate SSL certificate files. Useful when setting own files are necessary.

* `mysql_disable_binary_logging: false`  
Option to disable bin-log or not. Useful for replication.

* `nextcloud_cron_interval: 10`  
Option to set custom cron execution interval.

* `mysql_use_emoji: true`  
Makes database "emoji-ready" (UTF8 4-byte support, barracuda)

* `mysql_transaction_level: READ-COMMITTED`  
Sets MySQL transaction level. Empty value sets level to default.

* `mysql_replication: true`  
Sets `log-bin`, `server_id`, `replicate-do-db`, and `bind-address` in MySQL config file.

* `nextcloud_mail_update_cfg: true`  
Updates mail configuration with following options:

```
nextcloud_mail_smtpmode: smtp
nextcloud_mail_smtpauthtype: LOGIN
nextcloud_mail_from_address: your-address
nextcloud_mail_domain: your-domain
nextcloud_mail_smtphost: smtp.your-smtp.org
nextcloud_mail_smtpport: 465
nextcloud_mail_smtpauth: 1
nextcloud_mail_smtpname: your-name@your-domain.com
nextcloud_mail_smtppassword: your-password
nextcloud_mail_smtpsecure: ssl
```

* Updates `datadirectory` on deployment.

* Sets `skeletondirectory` to empty value.

* `nextcloud_enable_previews: false`  
Enables / disables (as in hardening guidance) previews

rbicker.nextcloud
=================

* install nextcloud (12) on centos 7
* install dependencies: nginx, php7.1, redis, mariadb
* generate ssl cert (self signed) if nextcloud_use_https is true
* follow best practises, performance tuning 
* Nextcloud's updater.phar can be used to update the instance to the latest version

Important:
* php version has been upgraded from 7.0 to 7.1, if you would like to update a server which has been installed by an older version of this role, run "yum remove -y php70w\*"
* upgrading nextcloud was removed from this role

Requirements
------------

* Currently only tested with centos 7.3

Role Variables
--------------

```
nextcloud_domain: nextcloud.mydomain.com # domain used in nginx and nextcloud version (REQUIRED)
mysql_root_pw: secret # root password for 
nextcloud_repo_url: https://download.nextcloud.com/server/releases # where to get the nextcloud archive
nextcloud_version: 12.0.4 # version to install (or upgrade to)
nextcloud_use_https: true # set to false if you want to run your instance behind a loadbalancer with ssl-termination
nextcloud_ssl_cert: /etc/nginx/nextcloud.crt # self-signed ssl cert path
nextcloud_ssl_key: /etc/nginx/nextcloud.key # ssl key path
nextcloud_ssl_subject: '/C=CH/ST=Lucerne/L=Lucerne/O=/CN={{ nextcloud_domain }}' # subject for ssl cert
nextcloud_working_dir: /nextcloud # directory for storing scripts
nextcloud_web_root: /var/www/nextcloud # web root 
nextcloud_data_root: '{{ nextcloud_working_dir }}/data'
nextcloud_backup_dir: '{{ nextcloud_working_dir }}/backup'
nextcloud_admin_user: admin # nextcloud admin username
nextcloud_admin_pw: admin # nextcloud admin password
nextcloud_mysql_db: nextcloud # name of nextcloud mysql db
nextcloud_mysql_user: nextcloud # username for nextcloud mysql db
nextcloud_mysql_pw: nextcloud  # password for nextcloud mysql db
nextcloud_upgrade: false # upgrade instance if given nextcloud_version does not match the installed version
nextcloud_hsts_options: max-age=15768000; includeSubDomains; preload; # if set, hsts will be enabled with the given options
nextcloud_upgrade: false # if set to true, nextcloud's updater.phar is run to upgrade nextcloud to the latest version

```

Example Playbook
----------------

```
- hosts: servers
  roles:
      - { role: rbicker.nextcloud, nextcloud_domain: nextcloud.mydomain.com }
```

License
-------

BSD

