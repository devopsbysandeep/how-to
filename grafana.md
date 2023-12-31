Grafana
nmcli connection modify "enp0s3" ipv4.method manual ipv4.address 192.168.1.58/24 ipv4.dns 8.8.8.8 ipv4.gateway 192.168.1.1
hostnamectl set-hostname grafana.lab.dev

Moreover, we can also install Grafana by using it's official yum repository. Therefore, we are adding Grafana yum repository in our Linux server as follows.
```bash
vi /etc/yum.repos.d/grafana.repo
```

and add following directives there in.

```bash
[grafana]
name=grafana
baseurl=https://packages.grafana.com/oss/rpm
repo_gpgcheck=1
enabled=1
gpgcheck=1
gpgkey=https://packages.grafana.com/gpg.key
sslverify=1
sslcacert=/etc/pki/tls/certs/ca-bundle.crt
````

Build cache for yum repositories.
```bash
yum makecache fast
```

Installing Grafana on CentOS 7:
Now, we can install Grafana with the help of yum command.
```bash
yum install -y grafana
```

Enable and start Grafana service.
```
systemctl enable --now grafana-server.service
```
Created symlink from /etc/systemd/system/multi-user.target.wants/grafana-server.service to /usr/lib/systemd/system/grafana-server.service.
Allow Grafana default service port i.e. 3000/tcp in Linux firewall.
```bash
firewall-cmd --permanent --add-port=3000/tcp
firewall-cmd --reload
```
Open URL http://192.168.1.58:3000/ in a web browser.

01-grafana-web-ui-login

The default user/password for Grafana is admin/admin.

02-grafana-change-password

Because, we are login for the first time, therefore, Grafana prompt us to change the default password of admin user.

03-grafana-home-dashboard
