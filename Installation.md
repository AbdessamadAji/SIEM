# Installing Elasticsearch, Logstash, Kibana

## Install Necessary Utilities
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install apt-transport-https curl gnupg -y
```

## Add Elastic Repository
```bash
curl -fsSL https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elastic.gpg
echo "deb [signed-by=/usr/share/keyrings/elastic.gpg] https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-8.x.list
sudo apt update
```

## Install Elasticsearch
```bash
sudo apt install elasticsearch -y
```

## Tune JVM
```bash
sudo nano /etc/elasticsearch/jvm.options
```

Then set:
```
-Xms1g
-Xmx1g
```

## Configure Elasticsearch Network
```bash
sudo nano /etc/elasticsearch/elasticsearch.yml
```

Add:
```yaml
network.host: 0.0.0.0
discovery.type: single-node
```

Comment this:
```yaml
# cluster.initial_master_nodes:
```

## Start Elasticsearch
```bash
sudo systemctl daemon-reload
sudo systemctl enable elasticsearch
sudo systemctl start elasticsearch
```

## Install Kibana
```bash
sudo apt install kibana -y
```

Edit:
```bash
sudo nano /etc/kibana/kibana.yml
```

Set:
```yaml
server.host: "0.0.0.0"
elasticsearch.hosts: ["http://localhost:9200"]
```

Start Kibana:
```bash
sudo systemctl enable kibana
sudo systemctl start kibana
```

## Install Logstash
```bash
sudo apt install logstash -y
sudo systemctl enable logstash
sudo systemctl start logstash
```

## Validation
```bash
systemctl status elasticsearch
systemctl status kibana
systemctl status logstash
```

<img width="1920" height="947" alt="image" src="https://github.com/user-attachments/assets/4c578527-1d97-40f8-a6da-8a4099089e78" />

# Installing DVWA on Ubuntu Desktop

## Install DVWA on Ubuntu Desktop

### Update System
```bash
sudo apt update && sudo apt upgrade -y
```

### Install Web Stack + Database + Git
```bash
sudo apt install apache2 mariadb-server php php-mysqli php-gd libapache2-mod-php git -y
```

### Enable Apache
```bash
sudo systemctl enable apache2
sudo systemctl start apache2
```

### Download DVWA
```bash
cd /var/www/html
sudo git clone https://github.com/digininja/DVWA.git
sudo chown -R www-data:www-data DVWA
sudo chmod -R 755 DVWA
cd DVWA/config
sudo cp config.inc.php.dist config.inc.php
sudo nano config.inc.php
```

Set:
```php
$_DVWA['db_user'] = 'admin';
$_DVWA['db_password'] = 'password';
```

### Create Database
```bash
sudo mysql
```
```sql
CREATE DATABASE dvwa;
CREATE USER 'admin'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON dvwa.* TO 'admin'@'localhost';
FLUSH PRIVILEGES;
exit;
```

<!-- pic here -->

### Validation
```
http://192.168.56.104/DVWA/index.php
```

<img width="1836" height="969" alt="Screenshot from 2026-02-21 01-21-57" src="https://github.com/user-attachments/assets/f08b6d94-a005-4b2d-a3a4-84578c9e5719" />
