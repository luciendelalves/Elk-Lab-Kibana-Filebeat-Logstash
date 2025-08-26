# Dashboards no Kibana (02-kibana-dashboards.md)
> Painéis básicos usando o Kibana Lens para monitorar falhas de autenticação SSH.

```bash
# =========================
# KIBANA — DISCOVER
# =========================
# Ajuste o período para "Last 24 hours" (ou maior).
# Use a query KQL para filtrar falhas de login SSH:
event.dataset:"system.auth" and message:"*Failed password*"

# =========================
# DASHBOARD — PAINEL A
# =========================
# Nome: SSH failures over time
# Visualização: Gráfico de linhas
# Métrica: Count
# Eixo X: Date histogram em @timestamp
# Filtro KQL (aplicar no painel):
event.dataset:"system.auth" and message:"*Failed password*"

# =========================
# DASHBOARD — PAINEL B
# =========================
# Nome: Top source IP (SSH failures)
# Visualização: Barras ou Pizza
# Métrica: Count
# Quebrar por: Top values of source.ip (10)
# Mesmo filtro KQL acima.

# =========================
# DASHBOARD — PAINEL C
# =========================
# Nome: SSH failures by host
# Visualização: Barras
# Métrica: Count
# Quebrar por: Top values of host.hostname
# Mesmo filtro KQL acima.

# =========================
# MONTAR DASHBOARD FINAL
# =========================
# 1. Menu ≡ → Dashboard → Create dashboard
# 2. Add from library → selecione os 3 painéis criados
# 3. Salve como: System Auth — SSH Failures

## Exemplo de Dashboard

![Dashboard SSH Failures](img/dashboard-ssh-failures.png)

