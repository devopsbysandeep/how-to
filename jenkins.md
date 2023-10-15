Jenkins
```bash
nmcli connection modify "enp0s3" ipv4.method manual ipv4.address 192.168.1.38/24 ipv4.dns 8.8.8.8 ipv4.gateway 192.168.1.1
```
```bash
Jenkins.lab.dev
```
```bash
sudo wget -O /etc/yum.repos.d/jenkins.repo \
    https://pkg.jenkins.io/redhat-stable/jenkins.repo
```
```bash
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
```
```bash
sudo yum upgrade
```
# Add required dependencies for the jenkins package
```bash
sudo yum install java-11-openjdk
```
```bash
sudo yum install jenkins
```
```bash
sudo systemctl daemon-reload
```
```bash
systemctl start jenkins
```
```bash
systemctl enable jenkins
```
```bash
systemctl status jenkins
```
```bash
firewall-cmd --service=jenkins --new-service=jenkins
firewall-cmd --service=jenkins  --service=jenkins --set-short="Jenkins ports"
firewall-cmd --service=jenkins  --service=jenkins --set-description="Jenkins port exceptions"
firewall-cmd --service=jenkins  --service=jenkins --add-port=8080/tcp
firewall-cmd --service=jenkins --add-service=jenkins
firewall-cmd --zone=public --add-service=http --permanent
firewall-cmd --reload
```bash


errar: Unit jenkins.service has finished shutting down

alternatives --config java  set java



