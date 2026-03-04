# Key Concepts and Lessons Learned

## Storage and Disk Management

### LVM (Logical Volume Manager)

Creates a flexible storage layer so disks can be resized live without reinstalling the OS.

### VG (Volume Group)

A storage pool made from physical disks.

### LV (Logical Volume)

The actual "disk" your OS uses (acts like a partition).

### Filesystem Resize

After LV grows, filesystem still thinks old size. `resize2fs` makes the OS see the new space.

**Key Takeaway**: VM disk ≠ usable disk.

## Networking

Ubuntu Server networking = Netplan, not dhclient.

## Elasticsearch

Elasticsearch 8 = HTTPS + authentication by default.

**In case of errors, always check logs.**

## Security and Package Management

### gnupg

Used to verify software authenticity.

## Java Virtual Machine (JVM)

### -Xms1g

Initial JVM heap size = 1 GB.

### -Xmx1g

Maximum JVM heap size = 1 GB.

### JVM Heap Size

The amount of RAM reserved for Java applications to store objects and data.

### JVM (Java Virtual Machine)

Runtime environment that executes Elasticsearch.

### cluster.initial_master_nodes

Used ONLY for multi-node clusters to elect first master.

## ELK Stack Components

### Elasticsearch

Stores and indexes logs (the database).

### Logstash

Parses raw logs into structured fields. Transforms raw log → `source.ip`, `username`, `url` (the transformer).

### Kibana

Dashboards, searches, alerts, and detection rules.

### Filebeat

Lightweight agent on victim machine. Ships logs to Logstash/Elasticsearch (the collector).

## Data Flow
```
Victim logs
   ↓
Filebeat
   ↓
Logstash (parse)
   ↓
Elasticsearch (store)
   ↓
Kibana (visualize + detect)
```

## Filebeat

- Must run as root
- Reads files locally BEFORE sending

## Data Pipeline Summary

- **Filebeat** ships
- **Logstash** parses
- **Elasticsearch** stores
- **Kibana** visualizes and detects

## Logstash

### Pipeline

Pipeline = input → filter → output

### Grok

Converts raw text into structured fields.

Grok is a Logstash filter plugin and pattern language. It lives inside Logstash.

**Example:**
```
192.168.1.5 GET /DVWA/login.php 200
```

Becomes:
```
source_ip: 192.168.1.5
url: /DVWA/login.php
status: 200
```

## Detection Best Practices

- **Detect Techniques, Not Exact Payloads**
- **Good filtering reduces False Positives**
- **Elasticsearch Has Limits** – always optimize queries
- **Encoding and filter bypassing breaks naive detection**
