P1- Ubuntu showed only ~19–34 GB although VM disk was set to 70 GB.
solution: 
  check free space : sudo vgdisplay
  extend logical volume: sudo lvextend -l +100%FREE /dev/ubuntu-vg/ubuntu-lv
  resize filesystem: sudo resize2fs /dev/ubuntu-vg/ubuntu-lv

P2- No Internet (Host-only Only)
solution:
  add second adapter : NAT
  configure netplan : sudo nano /etc/netplan/00-installer-config.yaml
  for nat adapter set : dhcp4: true
  apply : sudo netplan apply

P3- Elasticsearch failed to start:  cluster.initial_master_nodes is not allowed with discovery.type: single-node
solution:
  edit config : sudo nano /etc/elasticsearch/elasticsearch.yml
  comment : #cluster.initial_master_nodes: ["siem"]
  restart: sudo systemctl restart elasticsearch
  verify: sudo systemctl status elasticsearch

P4- curl Returned Empty Reply : Elastic 8 uses HTTPS + auth by default.
solution:
  reset elastic pwd: sudo /usr/share/elasticsearch/bin/elasticsearch-reset-password -u elastic
  then authenticate : curl -k -u elastic https://localhost:9200

  
