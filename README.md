# ELK Lab (Elasticsearch + Kibana + Logstash + Filebeat)

Laboratório em Ubuntu Server com ingestão via Filebeat → Logstash → Elasticsearch e visualização no Kibana.

---

## 🔍 Preview do Dashboard

![Dashboard SSH Failures](docs/img/dashboard-ssh-failures.png)

---

## Objetivos
- Centralizar e visualizar logs do sistema em um ambiente de laboratório.
- Criar dashboards básicos de falhas de autenticação SSH.
- Documentar a instalação, configuração e troubleshooting.

---

## Arquitetura
Filebeat → Logstash → Elasticsearch → Kibana

---

## ⚙️ Setup resumido
1. **Elasticsearch**
   - Config em `/etc/elasticsearch/elasticsearch.yml`
   - Teste: `curl http://127.0.0.1:9200`

2. **Kibana**
   - Config em `/etc/kibana/kibana.yml`
   - Acesso: `http://<IP_VM>:5601`

3. **Logstash**
   - Pipeline em `/etc/logstash/conf.d/01-beats.conf`
   - Porta Beats: `5044`

4. **Filebeat**
   - Config saída para Logstash em `/etc/filebeat/filebeat.yml`
   - Módulo **system** habilitado (`syslog` e `auth`)
   - Serviço ativo: `systemctl status filebeat`

---

## 🔍 Descoberta de eventos (Discover no Kibana)
Data view: **filebeat-***  
Filtro de tempo: **Last 15 minutes**  

### Query KQL utilizada
```kql
event.dataset:"system.auth" and (message:"*Failed password*" or message:"*Invalid user*")
```
![SSH Failures Discover](docs/img/discover-ssh-failures.png)

## 📊 Dashboards SSH no Kibana

Após a ingestão dos logs de autenticação do Ubuntu pelo Filebeat, criamos visualizações no **Kibana Lens** para monitorar tentativas de login SSH inválidas.

---

### 🔹 Visualizações criadas

1. **SSH_Failed_Logins_Over_Time**  
   - Tipo: Line chart  
   - Campos:  
     - Eixo X → `@timestamp` (Date histogram)  
     - Eixo Y → `Count of records`  
   - Filtro aplicado:  
     ```kql
     message : "*Failed password*" or message : "*Invalid user*"
     ```
   - Resultado: gráfico de linha mostrando a evolução das tentativas de login SSH inválidas ao longo do tempo.  
   ![SSH Failed Logins Over Time](docs/img/ssh_failed_logins.png)

2. **SSH_Top_Messages**  
   - Tipo: Bar chart  
   - Campos:  
     - Eixo X → `Top values of message.keyword`  
     - Eixo Y → `Count of records`  
   - Filtro aplicado:  
     ```kql
     message : "*Failed password*" or message : "*Invalid user*"
     ```
   - Resultado: gráfico de barras exibindo as principais mensagens capturadas do `auth.log` (ex.: `Invalid user`, `Failed password for root`, etc.).  
   ![SSH Top Messages](docs/img/ssh_top_messages.png)

Essas visualizações foram adicionadas ao **Dashboard “SOC_ELK_SSH_Dashboard”**, consolidando os gráficos para análise centralizada no Kibana.

---

## Serviços e portas
- Kibana: 5601
- Elasticsearch: 9200
- Logstash (Beats): 5044

---

## Documentação
- [Setup](docs/01-setup.md)
- [SSH Failures (Discover)](docs/02-discover-ssh-failures.md)
- [Dashboards no Kibana](docs/02-kibana-dashboards.md)
- [Troubleshooting](docs/03-troubleshooting.md)

