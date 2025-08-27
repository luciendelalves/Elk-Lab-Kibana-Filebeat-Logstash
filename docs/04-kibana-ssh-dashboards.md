## 📊 Dashboards SSH no Kibana

Após a ingestão dos logs de autenticação do Ubuntu pelo Filebeat, criamos visualizações no **Kibana Lens** para monitorar tentativas de login SSH inválidas.

### 🔹 Visualizações criadas

**1. SSH_Failed_Logins_Over_Time**  
- Tipo: Line chart  
- Campos:  
  - Eixo X → `@timestamp` (Date histogram, intervalo ajustado para 30m)  
  - Eixo Y → `Count of records`  
- Filtro aplicado: `message : "*Failed password*" or message : "*Invalid user*"`  
- Resultado: gráfico mostrando a evolução das tentativas de login SSH inválidas ao longo do tempo.  
![SSH Failed Logins Over Time](docs/img/SSH_Failed_Logins_Over_Time.png)

**2. SSH_Top_Messages**  
- Tipo: Bar chart  
- Campos:  
  - Eixo X → `Top values of message.keyword`  
  - Eixo Y → `Count of records`  
- Filtro aplicado: `message : "*Failed password*" or message : "*Invalid user*"`  
- Resultado: gráfico exibindo as principais mensagens capturadas do `auth.log` (ex.: `Invalid user`, `Failed password for root`, etc.).  
![SSH Top Messages](docs/img/SSH_Top_Messages.png)

---

## 📌 Dashboard consolidado
As duas visualizações foram adicionadas ao **Dashboard “SOC_ELK_SSH_Dashboard”**, centralizando a análise de falhas SSH no Kibana.

![Dashboard SSH Failures](../docs/img/dashboard-ssh-failures.png)

