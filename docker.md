# Anna Jordan - Docker Project
## Set Up
Power on VM \
ssh into class gateway then into personal VM 
## Install Docker
Install docker 
```
sudo apt update
sudo apt install ca-certificates curl gnupg lsb-release
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io

sudo usermod -aG docker admin
```
Install docker compose
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose
```
## WordPress
Create a directory for the project
```
mkdir dockWordPress
```
Switch into the project directory
```
cd dockWordPress
```
Create a file "docker-compose.yml"
```
touch docker-compose.yml
```
Add code to file to start Wordpress
```
echo "
version: "3.9"
    
services:
  db:
    image: mysql:5.7
    volumes:
      - db_data:/var/lib/mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: somewordpress
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress
    
  wordpress:
    depends_on:
      - db
    image: wordpress:latest
    volumes:
      - wordpress_data:/var/www/html
    ports:
      - "8000:80"
    restart: always
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
      WORDPRESS_DB_NAME: wordpress
volumes:
  db_data: {}
  wordpress_data: {}
  " >> docker-compose.yml
  ```
  *note: when I did this the quotations around the version number were removed. I fixed the issue by adding them back with the vi text editor* \
  Check that the file was written to with:
  ```
  cat docker-compose.yml
  ```
  Complete build
  ```
  docker-compose up -d
  ```
  
  
  
  
  
  
  
  
  
  
  
  
  
  
