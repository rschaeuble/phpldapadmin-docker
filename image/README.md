# phpLDAPadmin Docker Container

A docker container running phpLDAPadmin.


## Getting started

To download and start the docker container, run the following command:
`docker run --name my-phpldapadmin -d -p 80:80 rschaeuble/phpldapadmin:latest`

You can now open `http://localhost:80` in your web browser, which should then display phpLDAPadmin's web page.

Before phpLDAPadmin can be properly used, it needs to be configured though.


## Configuration

### LDAP URL

phpLDAPadmin needs to know the URL of the LDAP server. This is set using the environment variable
`LDAP_URL` (e.g. using the `--env` parameter of `docker run`). The URL can be specified using one of the following
formats:
* Hostname: `hostname.example.com`
* IP address: `127.0.0.1`
* LDAP URL: `ldap://hostname.example.com`
* LDAP URL (TLS): `ldaps://hostname.example.com`

In all cases, the port can be specified (e.g. `hostname.example.com:389`).

### LDAP_BASE

This is the name of the top-level entry in the LDAP database.

### LDAP_ADMIN

This is the DN of the LDAP admin, and is used solely for the purpose of prefilling the login page.
Can be set to an empty value.

### LDAP_CERTS

phpLDAPadmin can use secure connections (using TLS) to an LDAP server. If the LDAP server has a valid (non-self-signed) certificate, no further configuration is required.
In case a self-signed certificate is used, this needs to be provided. This is done by bind-mounting a certificate file
(in PEM format) into the container and setting the environment variable LDAP_CERTS to the path of this file.

### Example

This is an example configuration; please modify to suit your own circumstances:

```
LDAP_URL ldap://my-ldap-server.example.com
LDAP_BASE dc=example,dc=com
LDAP_ADMIN cn=admin,dc=example,dc=com
ENV LDAP_CERTS /data/ldap-server-certificate.crt
```
