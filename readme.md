# Sonarqube using LDAP
You can configure SonarQube authentication and authorization to an LDAP server (including the LDAP service of Active Directory).
This is a pratical example how sonarqube can connect with ldap to manage users using external authentication as LDAP.

## Important things
- Automatic synchronization of usernames and emails
- Automatic synchronization of relationships between users and groups (authorization)
- Ability to authenticate against both the external and the internal authentication systems. There is an automatic fallback to the SonarQube internal system if the LDAP server is down
- During the first authentication trial, if the user's password is correct, the SonarQube database is automatically populated with the new user. Each time a user logs into SonarQube, the username, the email, and the groups this user belongs to that are refreshed in the SonarQube database. You can choose to have group membership synchronized as well, but this is not the default
>[REFERENCE](https://docs.sonarqube.org/9.8/instance-administration/authentication/ldap/)<

## Setup
- Configure LDAP by editing `<SONARQUBE_HOME>/conf/sonar.properties` (in this case we used environ variables).
- Restart the SonarQube server and check the log file for:
INFO org.sonar.INFO Security realm: LDAP ...
INFO o.s.p.l.LdapContextFactory Test LDAP connection: OK
- Log in to SonarQube.
- On log out users will be presented with a login page (/sessions/login), where they can choose to log in as a technical user or a domain user by passing the appropriate credentials.
>[REFERENCE](https://docs.sonarqube.org/9.8/instance-administration/authentication/ldap/)<

## Tecnology
- Docker
- LDAP
## Containers
- [bitnami/openldap](https://hub.docker.com/r/bitnami/openldap)
- [sonarqube](https://hub.docker.com/_/sonarqube)
# How to test:
- run command
> docker-compose up
- you can test if ldap is listening using openldap package. (It will ask ldap password wich is  `adminpassword`)
> ldapsearch -x -b "dc=example,dc=org" -H ldap://127.0.0.1:1389 -D "cn=admin,dc=example,dc=org" -W
- you can also create/delete a user on [ldap cli](https://www.thegeekstuff.com/2015/02/openldap-add-users-groups/)
```
CREATE:
ldapadd -x -W -D "cn=admin,dc=example,dc=org" -H ldap://127.0.0.1:1389 -f chupetoide.ldif
ldappasswd -s chupetoide1 -W -D "cn=admin,dc=example,dc=org" -x "cn=chupetoide,ou=users,dc=example,dc=org" -H ldap://127.0.0.1:1389

#DELETE
ldapdelete -W -D "cn=admin,dc=example,dc=org" "cn=chupetoide,ou=users,dc=example,dc=org" -H ldap://127.0.0.1:1389
```
- access the sonarqube interface
> http://localhost:9000
- User and password should be `admin`
- After validate that user admin is working, you have to try access with ldap user wich is in openldap environment variables
> user: user01 and password: password01
- After log in with ldap user, as administrator you can see the ldap user on users tab at sonarqube configuration.

# REFERENCE LINKS
- [SONARQUBE LDAP ENV VARIABLES](https://docs.sonarqube.org/latest/setup-and-upgrade/configure-and-operate-a-server/environment-variables/#ldap-configuration)
- [SONARQUBE LDAP SETUP](https://docs.sonarqube.org/9.8/instance-administration/authentication/ldap/)
- [OPEN LDAP DOCKERHUB DOCS](https://hub.docker.com/r/bitnami/openldap/)
- [SONARQUBE DOCKERHUB DOCS](https://hub.docker.com/_/sonarqube)
- [LDAP CLI](https://www.thegeekstuff.com/2015/02/openldap-add-users-groups/)
- [LDAP UI MANAGER](https://hub.docker.com/r/wheelybird/ldap-user-manager)

