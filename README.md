sudo apt update
sudo apt install openjdk-8-jdk -y
java -version


wget https://dlcdn.apache.org/tomcat/tomcat-8/v8.5.100/bin/apache-tomcat-8.5.100.tar.gz
sudo tar -zxvf apache-tomcat-8.5.100.tar.gz -C /opt/
cd /opt/apache-tomcat-8.5.100

sudo adduser --system --group tomcat8

sudo chown -R tomcat8:tomcat8 /opt/apache-tomcat-8.5.100

cd /opt/apache-tomcat-8.5.100/bin
./startup.sh

apt install net-tools -y

netstat -lntp


sudo vi /etc/systemd/system/tomcat.service

[Unit]
Description=Apache Tomcat Web Application Container
After=network.target

[Service]
Type=forking
User=tomcat8
Group=tomcat8
Environment=CATALINA_PID=/opt/apache-tomcat-8.5.100/temp/tomcat.pid
Environment=CATALINA_HOME=/opt/apache-tomcat-8.5.100
Environment=CATALINA_BASE=/opt/apache-tomcat-8.5.100
Environment='CATALINA_OPTS=-Xms512M -Xmx1024M -server -XX:+UseParallelGC'
ExecStart=/opt/apache-tomcat-8.5.100/bin/startup.sh
ExecStop=/opt/apache-tomcat-8.5.100/bin/shutdown.sh

[Install]
WantedBy=multi-user.target


sudo systemctl daemon-reload

sudo systemctl start tomcat

sudo systemctl enable tomcat

sudo systemctl status tomcat

chown -R tomcat8:tomcat8 /opt/apache-tomcat-8.5.100/

sudo systemctl restart tomcat
-----------------------------------

sudo apt update

sudo apt install mysql-server -y

sudo systemctl start mysql
sudo systemctl enable mysql
sudo systemctl status mysql

#sudo mysql_secure_installation

sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf

[mysqld]
bind-address            = 0.0.0.0


sudo systemctl restart mysql

---------------------------------------------
# mysql

> create database studentapp;
> use studentapp;
> CREATE TABLE Students(student_id INT NOT NULL AUTO_INCREMENT,
	student_name VARCHAR(100) NOT NULL,
  student_addr VARCHAR(100) NOT NULL,
	student_age VARCHAR(3) NOT NULL,
	student_qual VARCHAR(20) NOT NULL,
	student_percent VARCHAR(10) NOT NULL,
	student_year_passed VARCHAR(10) NOT NULL,
	PRIMARY KEY (student_id)
);

CREATE USER 'student'@'%' IDENTIFIED BY 'student@1';

GRANT ALL PRIVILEGES ON studentapp.* TO 'student'@'%';

flush privileges;

---------------------------------------------------
vi /etc/mysql/mysql.conf.d/mysqld.cnf

require_secure_transport = OFF

Driver
cd /opt/apache-tomcat-8.5.100/lib
wget https://repo1.maven.org/maven2/mysql/mysql-connector-java/8.0.30/mysql-connector-java-8.0.30.jar

chown -R tomcat8:tomcat8 mysql-connector-java-8.0.30.jar

cd /opt/apache-tomcat-8.5.100/webapps

copy the war/jar file

cd /opt/apache-tomcat-8.5.100/conf

vim context.xml

<Resource name="jdbc/TestDB" auth="Container" type="javax.sql.DataSource"
               maxActive="50" maxIdle="30" maxWait="10000"
               username="student" password="student@1"
               driverClassName="com.mysql.cj.jdbc.Driver"
               url="jdbc:mysql://10.0.0.4:3306/studentapp?useSSL=false&amp;allowPublicKeyRetrieval=true"/>

sudo systemctl restart tomcat


==================================================================================================






