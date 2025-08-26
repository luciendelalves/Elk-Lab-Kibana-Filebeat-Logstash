# Troubleshooting (03-troubleshooting.md)
> Problemas comuns no ELK Lab e soluções rápidas.

```bash
# =========================
# ERROS COM APT/REPO
# =========================
# Erro: NO_PUBKEY ao instalar pacotes Elastic
# Solução: reimportar chave GPG e reconfigurar o repositório oficial.

# =========================
# DISCO CHEIO
# =========================
# Liberar espaço:
sudo apt-get clean
sudo journalctl --vacuum-time=2d
# Ou expandir o VDI do VirtualBox.

# =========================
# ELASTICSEARCH NÃO SOBE
# =========================
# - Verifique vm.max_map_count:
sudo sysctl -w vm.max_map_count=262144
# - Remova blocos xpack.* extras do elasticsearch.yml
# - Recrie elasticsearch.keystore se corrompido.

# =========================
# PORTA 9200 OCUPADA
# =========================
# Pode estar conflitando com Wazuh Indexer/OpenSearch.
# Solução: parar o serviço conflitante ou mudar http.port em elasticsearch.yml.

# =========================
# LOGSTASH NÃO ESCUTA NA 5044
# =========================
# Verifique:
ss -ltnp | grep 5044
# Se não aparecer, cheque erros em:
journalctl -u logstash -n 50 --no-pager

# =========================
# FILEBEAT SEM FILESETS
# =========================
# Erro: "no enabled filesets"
# Solução: habilitar system.yml
sudo filebeat modules enable system
