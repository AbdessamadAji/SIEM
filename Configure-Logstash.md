# create Logstash pipline:
sudo nano /etc/logstash/conf.d/siem.conf
input {
  beats {
    port => 5044
  }
}

filter {
  if [log][file][path] =~ "auth.log" {
    grok {
      match => { "message" => "%{SYSLOGTIMESTAMP:timestamp} %{HOSTNAME:hostname} sshd\[%{NUMBER:pid}\]: %{DATA:ssh_message}" }
    }
  }

  if [log][file][path] =~ "access.log" {
    grok {
      match => { "message" => "%{IPORHOST:source_ip} - - \[%{HTTPDATE:timestamp}\] \"%{WORD:method} %{URIPATHPARAM:url} HTTP/%{NUMBER:http_version}\" %{NUMBER:status} %{NUMBER:bytes}" }
    }
  }
}

output {
  elasticsearch {
    hosts => ["https://localhost:9200"]
    user => "elastic"
    password => "<ELASTIC_PASSWORD>"
    ssl => true
    ssl_certificate_verification => false
    index => "siem-%{+YYYY.MM.dd}"
  }

  stdout { codec => rubydebug }
}

#should replace password => "<ELASTIC_PASSWORD>"

#what this does:  Logstash opens TCP port 5044 and waits for Filebeat.
Listens on 5044
Accepts Filebeat events
Parses:
SSH logs
Apache access logs
Sends parsed events to Elasticsearch over HTTPS.Prints events to terminal (debug)
#test logstash config
sudo /usr/share/logstash/bin/logstash --config.test_and_exit -f /etc/logstash/conf.d/siem.conf
#start logstash:
sudo systemctl restart logstash
sudo systemctl status logstash

#Verify Logstash is listening
sudo ss -tulnp | grep 5044

# on ubuntu Desktop:
cd /opt/filebeat
sudo ./filebeat -e  #Runs Filebeat in foreground.

# on siem:
curl -k -u elastic https://localhost:9200/_cat/indices  #Shows all Elasticsearch indices.
*pic here
