# Jenkins Installation
* Can be installed on any OS but for this demo we use ubuntu server
> Make sure the rquired rules are in place to allow ssh (port 20 ) and port 8080 (default port of jenkins to access in browser, can be modified in jenkins configuration if required)

### Jenkins main link
```
https://www.jenkins.io/
```
### Link to install on Ubuntu
```
https://www.jenkins.io/doc/book/installing/linux/#debianubuntu
```
> Before installing Jenkins, we need to install Java or JDK

* Installing JDK
```
apt update
```
```
apt install openjdk-21-jdk -y
```

* Start [_Jenkins installaition_](#link-to-install-on-ubuntu)
```
sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
```
```
echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
```
```
sudo apt-get update
```
```
sudo apt-get install jenkins
```

* Jenkins directory -- everything it needs/creates is in this folder, except logs (/var/log)
```
ls /var/lib/jenkins
```
> it also has the 'secrets' folder (/var/lib/jenkins/secrets/initialAminPassword) which has the initial password used to login for the first time. After this we need to set our own password



