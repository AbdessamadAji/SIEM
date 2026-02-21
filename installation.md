#Installing elasricsearch , logstash, kibana

# install necessary utilities
sudo apt update && sudo apt upgrade -y
sudo apt install apt-transport-https curl gnupg -y

#Add Elastic repository
curl -fsSL https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elastic.gpg

echo "deb [signed-by=/usr/share/keyrings/elastic.gpg] https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-8.x.list

sudo apt update

#Install Elasticsearch
sudo apt install elasticsearch -y

#Tune JVM
sudo nano /etc/elasticsearch/jvm.options
# then set
-Xms1g
-Xmx1g

#Configure Elasticsearch network
sudo nano /etc/elasticsearch/elasticsearch.yml
# add
network.host: 0.0.0.0
discovery.type: single-node
# comment this
cluster.initial_master_nodes:

#Start Elasticsearch
sudo systemctl daemon-reload
sudo systemctl enable elasticsearch
sudo systemctl start elasticsearch

#Install Kibana
sudo apt install kibana -y
#edit
sudo nano /etc/kibana/kibana.yml
#set
server.host: "0.0.0.0"
elasticsearch.hosts: ["http://localhost:9200"]
# start kibana
sudo systemctl enable kibana
sudo systemctl start kibana

#Install Logstash
sudo apt install logstash -y
sudo systemctl enable logstash
sudo systemctl start logstash

#validation:
systemctl status elasticsearch
systemctl status kibana
systemctl status logstash


# installing dvwa on Ubuntu Desktop:
Install DVWA on Ubuntu Desktop:
#Update system
sudo apt update && sudo apt upgrade -y
#Install web stack + database + git
sudo apt install apache2 mariadb-server php php-mysqli php-gd libapache2-mod-php git -y
#Enable Apache
sudo systemctl enable apache2
sudo systemctl start apache2
# verification
pic here
#Download DVWA
cd /var/www/html
sudo git clone https://github.com/digininja/DVWA.git
sudo chown -R www-data:www-data DVWA
sudo chmod -R 755 DVWA

cd DVWA/config
sudo cp config.inc.php.dist config.inc.php
sudo nano config.inc.php

# set
$_DVWA['db_user'] = 'admin';
$_DVWA['db_password'] = 'password';

#Create database
sudo mysql
CREATE DATABASE dvwa;

CREATE USER 'admin'@'localhost' IDENTIFIED BY 'password';

GRANT ALL PRIVILEGES ON dvwa.* TO 'admin'@'localhost';

FLUSH PRIVILEGES;

exit;

* pic here
http://192.168.56.104/DVWA/index.php
*pic here

#On Ubuntu Desktop , Install Filebeat
sudo apt update
sudo apt install filebeat -y
#Enable log modules

#validation:
cd ~
curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-8.19.11-linux-x86_64.tar.gz
tar xzvf filebeat-8.19.11-linux-x86_64.tar.gz
sudo mv filebeat-8.19.11-linux-x86_64 /opt/filebeat
cd /opt/filebeat



# edit Filebeat config:
sudo nano /opt/filebeat/filebeat.yml
#Disable Elasticsearch output
comment:
output.elasticsearch:
  hosts: ["localhost:9200"]
#Enable Logstash output
add:
output.logstash:
  hosts: ["192.168.56.103:5044"]
#Enable system + apache logs
# add
filebeat.inputs:
- type: filestream
  enabled: true
  paths:
    - /var/log/auth.log
    - /var/log/apache2/access.log
#Fix ownership
sudo chown root:root /opt/filebeat/filebeat.yml
# test config:
sudo ./filebeat test config

