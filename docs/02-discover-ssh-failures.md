# Descoberta de Falhas SSH no Kibana (Discover)

Este documento descreve como filtrar e visualizar eventos de falhas de autenticação SSH utilizando o Kibana.

---

## 🎯 Objetivo
- Detectar tentativas de login falhas no SSH (`Failed password`)  
- Identificar acessos com usuários inválidos (`Invalid user`)  
- Validar que os logs do **Filebeat → Logstash → Elasticsearch → Kibana** estão funcionando corretamente  

---

## 🔍 Query utilizada no Discover

Data view selecionado: **filebeat-***  
Filtro de tempo: **Last 15 minutes**  

```kql
event.dataset:"system.auth" and (message:"*Failed password*" or message:"*Invalid user*")
```

![SSH Failures Discover](docs/img/discover-ssh-failures.png)

