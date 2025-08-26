# ELK Lab (Elasticsearch + Kibana + Logstash + Filebeat)

Laboratório em Ubuntu Server com ingestão via Filebeat → Logstash → Elasticsearch e visualização no Kibana.

## Serviços e portas
- Kibana: 5601 | Elasticsearch: 9200 | Logstash (Beats): 5044

## Passos rápidos
1. Crie o Data View `filebeat-*` em **Stack Management → Data Views** (timestamp: `@timestamp`).
2. **Discover:** ajuste o tempo (Last 24 hours) e teste KQL:
   `event.dataset:"system.auth" and message:"*Failed password*"`
3. **Dashboard (Lens):**
   - SSH failures over time (Count x Date histogram @timestamp)
   - Top source IP (Count por top values de `source.ip`)
   - SSH failures by host (Count por top values de `host.hostname`)

