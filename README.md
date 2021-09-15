`MantisBT` is an open source issue tracker that provides
a delicate balance between simplicity and power.

## Example docker-compose.yml
The examples to start up mantisbt. Adapt for your server.

```
version: '2.0'
services:
  mantisbt:
    container_name: mantisbt
    image: [docker_image]
    ports:
      - "8989:80"
    links:
      - mysql
    restart: always
    volumes:
      - ./config:/var/www/html/config
      - ./plugins:/plugins:ro
      - ./custom_config:/custom_config:ro

  mysql:
     container_name: mariadb
     image: mariadb:latest
     environment:
       - MYSQL_ROOT_PASSWORD=root
       - MYSQL_DATABASE=bugtracker
       - MYSQL_USER=mantisbt
       - MYSQL_PASSWORD=mantisbt
     restart: always
     volumes:
       - ./mysql:/var/lib/mysql
```

> You can use `mysql`/`postgres` instead of `mariadb`.

## Install

```
$ firefox http://localhost:8989/admin/install.php
>>> username: administrator
>>> password: root
```

```
==================================================================================
Installation Options
==================================================================================
Type of Database                                        MySQL/MySQLi
Hostname (for Database Server)                          mysql
Username (for Database)                                 mantisbt
Password (for Database)                                 mantisbt
Database name (for Database)                            bugtracker
Admin Username (to create Database if required)         root
Admin Password (to create Database if required)         root
Print SQL Queries instead of Writing to the Database    [ ]
Attempt Installation                                    [Install/Upgrade Database]
==================================================================================
```

## Email

Append following to `/srv/mantis/config/config_inc.php`

```
$g_phpMailer_method = PHPMAILER_METHOD_SMTP;
$g_administrator_email = 'admin@example.org';
$g_webmaster_email = 'webmaster@example.org';
$g_return_path_email = 'mantisbt@example.org';
$g_from_email = 'mantisbt@example.org';
$g_smtp_host = 'smtp.example.org';
$g_smtp_port = 25;
$g_smtp_connection_mode = 'tls';
$g_smtp_username = 'mantisbt';
$g_smtp_password = '********';
```

## LDAP

Append following to `/srv/mantis/config/config_inc.php` for LDAP
authentication against an Active Directory server:

```
$g_login_method = LDAP;
$g_ldap_server = 'ldap://dc.example.com';
$g_ldap_root_dn = 'dc=example,dc=com';
$g_ldap_bind_dn = 'cn=readuser, dc=example, dc=com';
$g_ldap_bind_passwd = 'geheim123';
$g_ldap_organization = '';
$g_use_ldap_email = ON;
$g_use_ldap_realname = ON;
$g_ldap_protocol_version = 3;
$g_ldap_follow_referrals = OFF;
$g_ldap_uid_field = 'sAMAccountName';
```

## Disabling admin folder

MantisBT recommends to disable access to the admin folder once the initial
configuration has been done.

To do so, add `disable_admin` file in `custom_config` directory and restart the container

## PHP settings

To use custom `php.ini`, add `php.ini` file in `custom_config` directory and restart the container

## Post script

To run script after container start up, add `post_script` in `custom_config` directory and restart the container

## Mantisbt plugins settings

To add mantisbt plugins, create `plugins` directory and add plugin directories
in `plugins` directory, restart the container

## Mantisbt pages customize

To customize mantisbt pages

1. create `patches` directory in `custom_config` directory
2. put matisbt page files in `patches` directory, if files are in `core` or
   other directories, you need to create the same directories and add files in
   the same directories
3. restart the container

