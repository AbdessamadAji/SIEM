# Mini SIEM Lab

## Project Overview

This project builds a simple SIEM lab to understand how logs become detections.

The goal is to:

- Collect system and web logs  
- Centralize them  
- Analyze them  
- Write basic detection rules  
- Trigger alerts  
- Visualize activity  

Attacks are generated against a vulnerable web application to produce real telemetry.  
All detections are written manually.

This project focuses on learning how SIEM works internally, not on using prebuilt security rules.

---

## What This Project Is

A small SOC-style lab made of three virtual machines:

- Attacker machine  
- SIEM server (Ubuntu running Elastic Stack)  
- Victim machine (Ubuntu running DVWA + Filebeat)  

The attacker performs scans and web attacks.  
The victim generates logs.  
The SIEM collects, processes, and analyzes those logs.

---

## Tools Used

### Elasticsearch

Stores all collected logs and makes them searchable.

---

### Logstash

Parses raw logs (SSH and web logs) and extracts useful fields such as:

- IP address  
- Username  
- URL  
- Status code  

---

### Kibana

Used to:

- View logs  
- Build dashboards  
- Create detection rules  
- Generate alerts  

---

### Filebeat

Runs on the victim machine and sends log files to Logstash.

---

### DVWA (Damn Vulnerable Web Application)

Provides an intentionally vulnerable web application so attacks can be safely tested.

---

## Purpose

The overall purpose of this project is to practice the full detection workflow:

attack → logs → parsing → storage → analysis → detection → alert

