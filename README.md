# Open Clinic

Open clinic project extention based on `http://openclinic.sourceforge.net/`

## System Requirements

- 2CPUs, 4GB RAM, 30GB Storage, Ubuntu Os

## Installation
- Clone the repository to your server and move the zip file to the `/opt` directory.
```
apt update
apt install git
cd ~
git clone https://github.com/plydot/openclinic.package.git config
cd config
sudo chmod u+x wgetgdrive.sh
sudo ./wgetgdrive.sh 1O6fEwgFg3Gd1MkI1BDqTuPa58LFZHaLY application.zip
```
- Unzip the Installation package
```
sudo apt install unzip
sudo unzip application.zip
mv ~/application/openclinic /opt/openclinic
mv ~/application/sql /opt/sql
mv ~/application/mysql-apt-config_0.8.12-1_all.deb /opt/mysql-apt-config_0.8.12-1_all.deb
```
#### Dependencies
1. MySQL
Install MySQL version 5.7, use the file `mysql-apt-config_0.8.12-1_all.deb` in the installation folder.
- Follow this [guide](https://www.how2shout.com/linux/add-repository-to-install-mysql-5-7-on-ubuntu-20-04-lts-linux/) and install mysql 5.7 (excecute steps 1 -5 only and set `root` with no password).
- If this error ocuurs while running `apt update `
```
Err:5 http://repo.mysql.com/apt/ubuntu bionic InRelease
  The following signatures couldn't be verified because the public key is not available: NO_PUBKEY 467B942D3A79BD29
Reading package lists... Done
W: GPG error: http://repo.mysql.com/apt/ubuntu bionic InRelease: The following signatures couldn't be verified because the public key is not available: NO_PUBKEY 467B942D3A79BD29
E: The repository 'http://repo.mysql.com/apt/ubuntu bionic InRelease' is not signed.
N: Updating from such a repository can't be done securely, and is therefore disabled by default.
N: See apt-secure(8) manpage for repository creation and user configuration details.
```
Execute the following command (Note to replace the key with one on your console if different):
```
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 467B942D3A79BD29
sudo apt update
```
- Edit the `cnf` file.
```
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
```
Add this to the end of the file, save and restart mysql.
>> Contents
```
skip_ssl
lower_case_table_names=1
```
Restart mysql using `sudo service mysql restart`\
Create mysql user, databases and tables.\
`sudo mysql`
```
CREATE DATABASE `ikirezi` /*!40100 DEFAULT CHARACTER SET latin1 */;
CREATE DATABASE `ocadmin_dbo` /*!40100 DEFAULT CHARACTER SET latin1 */;
CREATE DATABASE `ocstats_dbo` /*!40100 DEFAULT CHARACTER SET latin1 */;
CREATE DATABASE `openclinic_dbo` /*!40100 DEFAULT CHARACTER SET latin1 */;

CREATE USER 'openclinic'@'%' IDENTIFIED BY '0pen';
GRANT Create user ON *.* TO 'openclinic'@'%';
GRANT Event ON *.* TO 'openclinic'@'%';
GRANT File ON *.* TO 'openclinic'@'%';
GRANT Process ON *.* TO 'openclinic'@'%';
GRANT Reload ON *.* TO 'openclinic'@'%';
GRANT Replication client ON *.* TO 'openclinic'@'%';
GRANT Replication slave ON *.* TO 'openclinic'@'%';
GRANT Show databases ON *.* TO 'openclinic'@'%';
GRANT Shutdown ON *.* TO 'openclinic'@'%';
GRANT Super ON *.* TO 'openclinic'@'%';
GRANT Create tablespace ON *.* TO 'openclinic'@'%';
GRANT Usage ON *.* TO 'openclinic'@'%';
GRANT Grant option ON *.* TO 'openclinic'@'%';
GRANT Alter ON openclinic_dbo.* TO 'openclinic'@'%';
GRANT Create ON openclinic_dbo.* TO 'openclinic'@'%';
GRANT Create view ON openclinic_dbo.* TO 'openclinic'@'%';
GRANT Delete ON openclinic_dbo.* TO 'openclinic'@'%';
GRANT Drop ON openclinic_dbo.* TO 'openclinic'@'%';
GRANT Grant option ON openclinic_dbo.* TO 'openclinic'@'%';
GRANT Index ON openclinic_dbo.* TO 'openclinic'@'%';
GRANT Insert ON openclinic_dbo.* TO 'openclinic'@'%';
GRANT References ON openclinic_dbo.* TO 'openclinic'@'%';
GRANT Select ON openclinic_dbo.* TO 'openclinic'@'%';
GRANT Show view ON openclinic_dbo.* TO 'openclinic'@'%';
GRANT Trigger ON openclinic_dbo.* TO 'openclinic'@'%';
GRANT Update ON openclinic_dbo.* TO 'openclinic'@'%';
GRANT Alter routine ON openclinic_dbo.* TO 'openclinic'@'%';
GRANT Create routine ON openclinic_dbo.* TO 'openclinic'@'%';
GRANT Create temporary tables ON openclinic_dbo.* TO 'openclinic'@'%';
GRANT Execute ON openclinic_dbo.* TO 'openclinic'@'%';
GRANT Lock tables ON openclinic_dbo.* TO 'openclinic'@'%';
GRANT Grant option ON openclinic_dbo.* TO 'openclinic'@'%';
GRANT Alter ON *.* TO 'openclinic'@'%';
GRANT Create ON *.* TO 'openclinic'@'%';
GRANT Create view ON *.* TO 'openclinic'@'%';
GRANT Delete ON *.* TO 'openclinic'@'%';
GRANT Drop ON *.* TO 'openclinic'@'%';
GRANT Grant option ON *.* TO 'openclinic'@'%';
GRANT Index ON *.* TO 'openclinic'@'%';
GRANT Insert ON *.* TO 'openclinic'@'%';
GRANT References ON *.* TO 'openclinic'@'%';
GRANT Select ON *.* TO 'openclinic'@'%';
GRANT Show view ON *.* TO 'openclinic'@'%';
GRANT Trigger ON *.* TO 'openclinic'@'%';
GRANT Update ON *.* TO 'openclinic'@'%';
GRANT Alter routine ON *.* TO 'openclinic'@'%';
GRANT Create routine ON *.* TO 'openclinic'@'%';
GRANT Create temporary tables ON *.* TO 'openclinic'@'%';
GRANT Execute ON *.* TO 'openclinic'@'%';
GRANT Lock tables ON *.* TO 'openclinic'@'%';
GRANT Grant option ON *.* TO 'openclinic'@'%';
FLUSH PRIVILEGES;
exit
```
Import initial data.
```
mysql ikirezi < /opt/sql/dump-ikirezi-202208111058.sql
mysql ocstats_dbo < /opt/sql/dump-ocstats_dbo-202208111058.sql
> ERROR 1146 (42S02) at line 803: Table 'ocadmin_dbo.admin' doesn't exist
mysql ocadmin_dbo < /opt/sql/dump-ocadmin_dbo-202208111058.sql
> ERROR 1146 (42S02) at line 2043: Table 'openclinic_dbo.oc_labels' doesn't exist
mysql openclinic_dbo < /opt/sql/dump-openclinic_dbo-202208111058.sql
> ERROR 1146 (42S02) at line 9710: Table 'ocadmin_dbo.labels' doesn't exist
mysql ocstats_dbo < /opt/sql/dump-ocstats_dbo-202208111058.sql
mysql ocadmin_dbo < /opt/sql/dump-ocadmin_dbo-202208111058.sql
mysql openclinic_dbo < /opt/sql/dump-openclinic_dbo-202208111058.sql
```

3. Java 
Install Java 8
```
sudo apt install openjdk-8-jdk
```
`java -version`
>> output
```
openjdk version "1.8.0_252"
OpenJDK Runtime Environment (build 1.8.0_252-8u252-b09-1ubuntu1-b09)
OpenJDK 64-Bit Server VM (build 25.252-b09, mixed mode)

```

#### Install application
Set up systemctl service to start and restart the application.
- create a files `/etc/systemd/system/openclinic.service` with the following contents.
>> contents
```
[Unit]
Description=openclinic
After=network.target

[Service]
Type=forking
User=root

Environment=CATALINA_PID=/opt/openclinic/tomcat.pid
# Environment=JAVA_HOME=/usr/bin/java
Environment=CATALINA_HOME=/opt/openclinic/
Environment=CATALINA_BASE=/opt/openclinic/
Environment='JAVA_OPTS=-Djava.awt.headless=true -Djava.security.egd=file:/dev/./urandom'

ExecStart=/opt/openclinic/bin/startup.sh
ExecStop=/opt/openclinic/bin/shutdown.sh
Restart=on-failure

[Install]
WantedBy=multi-user.target
```
Enable service and start service
```
sudo systemctl enable openclinic.service 
sudo service openclinic start
sudo service openclinic status
```
>> output
```
??? openclinic.service - openclinic
     Loaded: loaded (/etc/systemd/system/openclinic.service; enabled; vendor preset: enabled)
     Active: active (running) since Mon 2022-07-18 18:45:58 CEST; 1 weeks 4 days ago
   Main PID: 2730 (java)
      Tasks: 77 (limit: 9479)
     Memory: 1.4G
     CGroup: /system.slice/openclinic.service
             ??????2730 /usr/bin/java -Djava.util.logging.config.file=/opt/openclinic-package-v5.155.03/openclinic-SNAPSHOT-1.0.0/conf/logging.properties -Dja>

Warning: journal has been rotated since unit was started, output may be incomplete.
```
Other operations
```
sudo service openclinic stop
sudo service openclinic restart
```
The application is now accesible directly through the IP Address of your server.
i.e. `http://my-server-ip-address/openclinic/`
```
username: openclinic
password: whippet572
```

### MYSQL DUMP DB
`mysql database_name > database_name.sql`\
`mysql -u user -pPassword db_name > db_name.sql`

## UPDATE EXISTING
```
- replace `/webapps/openclinic/_common/header.jsp` #Remember to backup existing file
- replace `/webapps/onclinic/_common/start.jsp` #Remember to backup existing file
- replace `ROOT` directory #Remember to backup existing folder
```
#### Update Tables
```
mysql ocadmin_dbo < keys.sql
```

## Authors and acknowledgment
&copy; 2022 [Plydot Systems](https://plydot.com)

