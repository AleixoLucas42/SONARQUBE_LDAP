# Sonarqube using LDAP
You can configure SonarQube authentication and authorization to an LDAP server (including the LDAP service of Active Directory).
This is a pratical example how sonarqube can connect with ldap to manage users using external authentication as LDAP.

## Important things
- Automatic synchronization of usernames and emails
- Automatic synchronization of relationships between users and groups (authorization)
- Ability to authenticate against both the external and the internal authentication systems. There is an automatic fallback to the SonarQube internal system if the LDAP server is down
- During the first authentication trial, if the user's password is correct, the SonarQube database is automatically populated with the new user. Each time a user logs into SonarQube, the username, the email, and the groups this user belongs to that are refreshed in the SonarQube database. You can choose to have group membership synchronized as well, but this is not the default
>[REFERENCE](https://docs.sonarqube.org/9.8/instance-administration/authentication/ldap/)

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
- Windows server active directory
## Containers
- [sonarqube](https://hub.docker.com/_/sonarqube)
# How to test:
- run command
> docker-compose up
- access the sonarqube interface
> http://localhost:9000
- User and password should be `admin`
- After validate that user admin is working, you have to try access with ldap user of active directory
- After log in with ldap user, back to administrator and you can see the ldap user on users tab at sonarqube configuration.
- To associate user to a AD group, you have to [manually create a group](https://docs.sonarsource.com/sonarqube/9.9/instance-administration/authentication/overview/) with the same name on sonarqube interface.

# REFERENCE LINKS
- [SONARQUBE LDAP ENV VARIABLES](https://docs.sonarqube.org/latest/setup-and-upgrade/configure-and-operate-a-server/environment-variables/#ldap-configuration)
- [SONARQUBE LDAP SETUP](https://docs.sonarqube.org/9.8/instance-administration/authentication/ldap/)
- [OPEN LDAP DOCKERHUB DOCS](https://hub.docker.com/r/bitnami/openldap/)
- [SONARQUBE DOCKERHUB DOCS](https://hub.docker.com/_/sonarqube)
- [LDAP CLI](https://www.thegeekstuff.com/2015/02/openldap-add-users-groups/)
- [LDAP UI MANAGER](https://hub.docker.com/r/wheelybird/ldap-user-manager)

