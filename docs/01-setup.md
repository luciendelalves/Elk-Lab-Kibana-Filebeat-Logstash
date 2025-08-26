# Setup do ELK Lab (Ubuntu + Filebeat → Logstash → Elasticsearch → Kibana)
> Guia rápido em um único bloco. Linhas iniciadas com `#` são comentários.

# =========================
# REQUISITOS DO AMBIENTE
# =========================
# - Ubuntu Server 22.04 (VM VirtualBox)
# - 2 vCPU, 4–8 GB RAM, 40–60 GB disco
# - Portas: Kibana 5601 | Elasticsearch 9200 | Logstash 5044
# - Acesso ao Kibana: http://<IP_DA_VM>:5601  (NAT com port-forward ou Bridged)

# =========================
# ELASTICSEARCH (mínimo)
# =========================
# Arquivo: /etc/elasticsearch/elasticsearch.yml
cluster.name: elk-lab
node.name: elk-01
network.host: 127.0.0.1
http.port: 9200
discovery.type: single-node
xpack.security.enabled: false
path.data: /var/lib/elasticsearch
path.logs: /var/log/elasticsearch

# Comandos:
# (1) Ajuste kernel para ES
sudo sysctl -w vm.max_map_count=262144
# (2) Start e enable
sudo systemctl enable --now elasticsearch
# (3) Teste
curl -s http://127.0.0.1:9200

# =========================
# KIBANA
# =========================
# Arquivo: /etc/kibana/kibana.yml
server.host: "0.0.0.0"
elasticsearch.hosts: ["http://127.0.0.1:9200"]

# Comandos:
sudo systemctl enable --now kibana
# Acesso no navegador: http://<IP_DA_VM>:5601

# =========================
# LOGSTASH (pipeline Beats → ES)
# =========================
# Arquivo: /etc/logstash/conf.d/01-beats.conf
input { beats { port => 5044 } }
filter { }
output {
  elasticsearch {
    hosts => ["http://127.0.0.1:9200"]
    index => "%{[@metadata][beat]}-%{+YYYY.MM.dd}"
  }
}

# Comandos:
sudo systemctl enable --now logstash

# =========================
# FILEBEAT (módulo system)
# =========================
# Arquivo: /etc/filebeat/filebeat.yml (somente a saída para Logstash)
output.logstash:
  hosts: ["127.0.0.1:5044"]

# Arquivo: /etc/filebeat/modules.d/system.yml (habilitar syslog + auth)
- module: system
  syslog:
    enabled: true
  auth:
    enabled: true

# Comandos:
sudo filebeat modules enable system
sudo systemctl enable --now filebeat
sudo filebeat test output

# =========================
# VERIFICAÇÕES RÁPIDAS
# =========================
# Índices do ES (procure por filebeat)
curl -s http://127.0.0.1:9200/_cat/indices?v | grep filebeat
# Logstash escutando na 5044
ss -ltnp | grep 5044
# Logs do Filebeat (últimas linhas)
journalctl -u filebeat -n 50 --no-pager

# =========================
# KIBANA — DATA VIEW
# =========================
# No Kibana: Stack Management → Data Views → Create data view
# Name: filebeat-*     |     Timestamp field: @timestamp
# Depois, vá em Discover e ajuste o tempo para Last 24 hours.

# =========================
# KQL DE TESTE (Discover)
# =========================
# Falhas de autenticação SSH:
# event.dataset:"system.auth" and message:"*Failed password*"
