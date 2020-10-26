Nexus Repository Config
==================
## 1. Install docker
- sudo curl -sSL get.docker.io | sudo bash
## 2. Install docker-compose
- sudo curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
- sudo chmod +x /usr/local/bin/docker-compose
- sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
- docker-compose --version
## 3. Run
- docker-compose up -d
- docker-compose stop