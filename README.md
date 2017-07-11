NGINX Loadbalancer [![Build Status](https://travis-ci.org/wilmardo/ansible-role-nginx-loadbalancer.svg?branch=master)](https://travis-ci.org/wilmardo/ansible-role-nginx-loadbalancer)
=========

Ansible role to create letsencrypt certificates

Requirements
------------
None

Role Variables
--------------
There are several role variables, they can be set in the hosts_vars/group_vars:

### Variables for certificate requests

| Variable name             | Default value         | Description         |
| ------------------------- | --------------------- | ------------------- |
| letsencrypt_certificates  | undefined             | Dict contain domains to generate ssl certificates for, see the example playbook for the structure of the dict
| letsencrypt_email         | undefined             | Email to use for certificate requests, is also used for expiration notifications
| letsencrypt_webserver     | apache                | The webserver in use on the server, if none leave empty string
| letsencrypt_autoinstall   | yes                   | If true Certbot will get a certificate for you and edit your configuration automatically to serve it
| letsencrypt_certonly      | no                    | If true Certbot will get a certificate for you but no edit to your configuration
| letsencrypt_standalone    | no                    | The ACME request is done with a temporary webserver

### Variables for certbot

| Variable name                 | Default value     | Description         |
| ----------------------------- | ----------------- | ------------------- |
| letsencrypt_rsa_key_size      | 2048              | Letsencrypt RSA keysize, recommended to change to 4096 
| letsencrypt_extra_conf        | ""                | Could be used to append to cli.ini for option see certbot help

### Variables for letsencrypt cron job

| Variable name                    | Default value          | Description         |
| -------------------------------- | -----------------------| ------------------- |
| letsencrypt_auto_renew           | yes                    | Enables creation of a cron job to renew certificates when needed (certbot renew)
| letsencrypt_auto_renew_user      | {{ ansible_user }}     | User to run cron job with
| letsencrypt_auto_renew_hour      | random                 | Hour to set cron job to, is generated randomly and leave it preferably to reduce serverload at Lets Ecnrypt 
| letsencrypt_auto_renew_minute    | random                 | Minute to set cron job to, is generated randomly and leave it preferably to reduce serverload at Lets Ecnrypt

Dependencies
------------
None

Example Playbooks
----------------

### Example Playbook with Apache and autoinstall
The following playbook gives an example when using Apache and the autoinstall.

    - hosts: webservers    
      roles:
         - { role: wilmardo.letsencrypt,    letsencrypt_email: example@exaple.com,
                                            letsencrypt_certificates: [{ example.com: {domain: example.com} }]

### Example Playbook with Nginx and webroot
The following playbook gives an example when using Nginx with a webroot and a 4096 rsa_key_size

    - hosts: webservers    
      roles:
         - { role: wilmardo.letsencrypt,    letsencrypt_email: example@example.com,
                                            letsencrypt_certificates: [{ example.com: {domain: example.com, webroot:/home/apache/www/example.com}, example1.com: {domain: example1.com, , webroot:/home/apache/www/example1.com/} }]
                                            letsencrypt_rsa_key_size: 4096
                                            
### Example Playbook without webserver, verification through standalone
The following playbook gives an example when using no webserver (mailserver for example)

    - hosts: mailservers    
      roles:
         - { role: wilmardo.letsencrypt,    letsencrypt_email: example@example.com,
                                            letsencrypt_certificates: [{ example.com: {domain: mail.example.com }]
                                            letsencrypt_webserver: "",
                                            letsencrypt_standalone: yes
                                            
License
-------

BSD

Author Information
------------------

Wilmar den Ouden

https://wilmardenouden.nl
