update-crl
==========

Simple script for downloading and updating CRL (Certificate Revocation List)

If a CRL provider are unavaible or a CRL file malformed, script do not delete current CRL file. In addition, if a CRL file cant be refresh for a defined time, script delete expried CRL file.

Optional : Send a mail notification if update fail or if expiration. Can use a proxy.

Requirements
------------

Tested with lastest Red Hat EL7 and EL8. Require openssl obviously

Usage
-----

Basic variables :
- crl_dir : folder where CRL are download and symlink created, default /etc/pki/tls/ca-crls/
- crl_temp : name of the temp file used by wget, default temp.crl
- proxy_url : proxy used by wget, default http://{{ proxy_address }}:{{ proxy_port }}

CRL providers :
Foreach provider, add a line :
```
	declare -a arr=(
        "ExampleCA http://example.domain.fr/exampleCA.crl false"
        "titiCA http://ca.titi.fr/pem.crl true"
        )
```
- local_crl_delay : if a CRL file not refresh since x days, it is deleted, default 6 days

SMTP notifications :
- smtp_alert : enable or disable notifications, default true
- smtp_hostname : subject contain node hostname, default {{ inventory_hostname_short }}
- smtp_sender : mail sender, default {{ inventory_hostname_short }}@domain.fr
- smtp_dest : mail recipient, default {{ httpd_admin }}

Ansible
-------

In your ansible role, use it has a jinja template. Simply add a yaml array :
```
x509auth_cacrl :
  - name: 'exampleCA'
    url: 'http://example.domain.fr/exampleCA.crl'
  - name: 'toto'
    url: 'http://ca.toto.com/toto.crl'
  - name: 'titiCA' 
    url: 'http://ca.titi.fr/pem.crl'
    pem: 'true'
```

Others jinja variables can be configured :
- proxy_address and proxy_port if your server need to use a proxy with wget
- httpd_admin to set email address for SMTP notifications

License
-------

AGPLv3 

Author Information
------------------

Gilian GAMBINI @ DSI-CNRS
