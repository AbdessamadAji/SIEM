# Create Logstash Pipeline
```bash
sudo nano /etc/logstash/conf.d/siem.conf
```
```conf
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
```


What this does: 
- Logstash opens TCP port 5044 and waits for Filebeat.
- Listens on 5044
- Accepts Filebeat events
- Parses:
  - SSH logs
  - Apache access logs
- Sends parsed events to Elasticsearch over HTTPS
- Prints events to terminal (debug)

## Test Logstash Config
```bash
sudo /usr/share/logstash/bin/logstash --config.test_and_exit -f /etc/logstash/conf.d/siem.conf
```

## Start Logstash
```bash
sudo systemctl restart logstash
sudo systemctl status logstash
```

## Verify Logstash is Listening
```bash
sudo ss -tulnp | grep 5044
```

## On Ubuntu Desktop
```bash
cd /opt/filebeat
sudo ./filebeat -e  # Runs Filebeat in foreground
```

## On SIEM
```bash
curl -k -u elastic https://localhost:9200/_cat/indices  # Shows all Elasticsearch indices
```

<img width="811" height="381" alt="Screenshot from 2026-02-20 18-26-00" src="https://github.com/user-attachments/assets/d3dfd609-20f5-4247-8b6d-449ea802d65d" />

## Create Data View

Stack Management → Data Views → Create data view with name `siem`, and index `siem-*`
