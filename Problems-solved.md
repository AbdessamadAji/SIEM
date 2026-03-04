# Troubleshooting and Solutions

## P1 - Ubuntu Showed Only ~19–34 GB Although VM Disk Was Set to 70 GB

### Problem

Ubuntu showed only approximately 19–34 GB of disk space even though the VM disk was configured for 70 GB.

### Solution

Check free space:
```bash
sudo vgdisplay
```

Extend logical volume:
```bash
sudo lvextend -l +100%FREE /dev/ubuntu-vg/ubuntu-lv
```

Resize filesystem:
```bash
sudo resize2fs /dev/ubuntu-vg/ubuntu-lv
```

## P2 - No Internet (Host-only Only)

### Problem

VM had no internet connectivity with host-only adapter.

### Solution

Add second adapter: NAT

Configure Netplan:
```bash
sudo nano /etc/netplan/00-installer-config.yaml
```

For NAT adapter set:
```yaml
dhcp4: true
```

Apply changes:
```bash
sudo netplan apply
```

## P3 - Elasticsearch Failed to Start

### Problem

Error: `cluster.initial_master_nodes is not allowed with discovery.type: single-node`

### Solution

Edit config:
```bash
sudo nano /etc/elasticsearch/elasticsearch.yml
```

Comment out:
```yaml
#cluster.initial_master_nodes: ["siem"]
```

Restart:
```bash
sudo systemctl restart elasticsearch
```

Verify:
```bash
sudo systemctl status elasticsearch
```

## P4 - curl Returned Empty Reply

### Problem

Elastic 8 uses HTTPS and authentication by default.

### Solution

Reset Elastic password:
```bash
sudo /usr/share/elasticsearch/bin/elasticsearch-reset-password -u elastic
```

Then authenticate:
```bash
curl -k -u elastic https://localhost:9200
```

## P5 - POST Payloads Not Visible (Command Injection)

### Problem

POST request payloads were not being logged, making command injection detection impossible.

### Solution

- Initially tried `mod_dumpio` (noisy and unreliable)
- Implemented custom application logging in DVWA
- Created dedicated logging file
- Shipped logs via Filebeat

## P6 - XSS Encoded Payloads Not Detected

### Problem

Payloads were automatically URL encoded, example: `%3Csvg%20onload=alert(1)%3E`

### Solution

- Added encoded detection patterns (e.g., `%3csvg`)
- Adjusted rule to detect both plain and encoded variants

## P7 - Couldn't Build Visualizations by IP for Command Injection

### Problem

Unable to group or visualize command injection attempts by source IP address.

### Solution

Added Logstash grok filter by extracting `source_ip` and `dvwa.command`.
