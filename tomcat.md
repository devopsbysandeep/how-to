tomcat

nmcli connection modify "enp0s3" ipv4.method manual ipv4.address 192.168.1.49/24 ipv4.dns 8.8.8.8 ipv4.gateway 192.168.1.1

tomcat-qa.lab.dev

Install Tomcat
```
yum install tomcat
```
- This will install Tomcat and its dependencies, including Java.
- There are several additional packages which many users, particularly those who are new to Tomcat, will find useful. Install them with the command:
```
yum install tomcat-webapps tomcat-admin-webapps tomcat-docs-webapp tomcat-javadoc
```
This will install:

The Tomcat root webpage (tomcat-webapps)
The Tomcat Web Admin Manager (tomcat-admin-webapps)
The official online Tomcat documentation (tomcat-docs-webapp and tomcat-javadoc)
If your server is running Apache, stop it with the command:
```
systemctl stop httpd
```
Start Tomcat with the command:
```
systemctl start tomcat
```
And enable Tomcat to automatically start if the server is rebooted:
```
systemctl enable tomcat
```
You can verify that Tomcat is running by visiting the URL 
http://example.com:8080 or http://<your-IP>:8080 in a web browser. You will see the Tomcat welcome page, which includes links to the Tomcat documentation which you installed in the previous step.

Use the Tomcat Web Admin Manager
In order to use Tomcat's web management interface, you will need to create a user. Open the tomcat-users.xml file with the command:
```
vi /usr/share/tomcat/conf/tomcat-users.xml
```S
croll down to below the line which reads <tomcat-users> and add the information for your user account:
```
<user username="[username]" password="[password]" roles="manager-gui,admin-gui"/>
```
For example, to add the user ramesh with password Redhat@#309 this section will read:
```
<tomcat-users>
<user username="ramesh" password="Redhat@#309" roles="manager-gui,admin-gui"/>
```
Save and exit the file. Restart the Tomcat service for the changes to take effect:

```
firewall-cmd --zone=public --permanent --add-port=8080/tcp
firewall-cmd --reload
```
```
systemctl restart tomcat
```
In a browser, visit the URL http://example.com:8080 to see the Tomcat welcome page. Click the Manager App link.


