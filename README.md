# 06 Devops: Nexus Repository Manager
Repository demonstrates how to install and configure Nexus on a cloud server and upload applications

**Nexus** = Repository Manager to upload and store different build artifacts (different formats of artifacts - python, docker, java, .net  etc.) . 

- create a virtual machine on Digital Ocean Platform with 8GB of memory
- check if java is installed : *java --version*
- download Nexus installation package for Linux to /opt by using a `wget` command
  *wget https://download.sonatype.com/nexus/3/nexus-3.90.1-01-linux-x86_64.tar.gz*
- untar downloaded package *tar -zxvf nexus-3.90.1-01-linux-x86_64.tar.gz*
- service can be started directly from `nexus-3.90.1-01` directory under special user called `nexus`
- *adduser nexus* commands creates a new user and group
- ownership of `nexus-3.90.1-01` and `sonatype-work` must be changed to `nexus` user and group: *chown -R nexus:nexus nexus-3.90.1-01*
- in `/opt/nexus-3.90.1-01/bin/nexus` the variable is set: **run_as_user='nexus'**
- service can be start under `nexus` user: */opt/nexus-3.90.1-01/bin/nexus start &* in detached mode. 
- to check whether the service is running these binaries can be used: *netstat -tlpn* OR *ss -tulnp*
- login with username *admin* and password saved in `/opt/sonatype-work/nexus3/admin.password` is necessary

- In Nexus we can create a user having the permission to upload artifacts: `Settings-Security-Users` & `Settings-Security-Roles`

## Gradle Project
- in the `build.gradle` file specifies where is the artifact located `build/libs` and its format and URL of the repository in the repository manager
- insecured HTTP protocol can be allowed by *allowInsecureProtocol = true* in `build.gradle`
- in `gradle.properties` define the username and password to authenticate in Nexus
- in `settings.gradle` the *rootProject.name* can be specified. It will be used to generate the name of the `jar` file 
- *gradle build* builds the artifact as jar file
- *gradle publish* uploads a jar file generated in build/libs to remote repository `maven snapshots`

## Maven Project
- in `pom.xml` configure deploy plugin *<plugin> </plugin>*
- define a repository - URL and id
- in `home` directory the file `settings.xml` is save and that includes credentials to authenticate in nexus repository. 
- *mvn package* - creates an artifact as `jar` file
- *mvn deploy* - uploads artifact to remote repository

Using the groupid *<groupId>com.example</groupId>* artifacts can be groupped inside the repository
