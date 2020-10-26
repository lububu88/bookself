Nexus Repository Config
==================
## 1. Install docker
- sudo curl -sSL get.docker.io | sudo bash
- docker --version
## 2. Install docker-compose
- sudo curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
- sudo chmod +x /usr/local/bin/docker-compose
- sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
- docker-compose --version
## 3. Run
- docker-compose up -d
- docker-compose stop

## 4. Config Nexus Repository
- Nexus URL: http://IP_NEXUS:8081

## 5. Package for OS Linux
- Centos-Base: 

```javascript
cd /etc/yum.repo/
vim Centos-Base.repo
yum clean all
yum repolist
```
- EPEL: 
```javascript
cd /etc/yum.repo/
vim epel.repo
yum clean all
yum repolist
```
- Redhat 7
```javascript
cd /etc/yum.repo/
vim redhat-local.repo
yum clean all
yum repolist
```
- Oracle Linux 7
```javascript
cd /etc/yum.repo/
vim yum-local-ol7.repo
yum clean all
yum repolist
```
- Ubuntu 18.04
```javascript
cd /etc/apt/
vim ubuntu_sources_18.04.list
apt-get update
```
- Ubuntu 20.04
```javascript
cd /etc/apt/
vim ubuntu_sources_20.04.list
apt-get update
```

## 6. Config Docker using Nexus Repositoty to pull images
```javascript
cd /etc/docker
vim daemon.json
systemctl restart docker
```

## 7. Config Pip to Install package
```javascript
vim /etc/pip.conf
pip install name-package
```
