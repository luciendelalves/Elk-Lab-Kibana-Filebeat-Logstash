# ELK Lab (Elasticsearch + Kibana + Logstash + Filebeat)

Laboratório em Ubuntu Server com ingestão via Filebeat → Logstash → Elasticsearch e visualização no Kibana.

## Objetivos
- Centralizar e visualizar logs do sistema em um ambiente de laboratório.
- Criar dashboards básicos de falhas de autenticação SSH.
- Documentar a instalação, configuração e troubleshooting.

## Arquitetura
Filebeat → Logstash → Elasticsearch → Kibana

## Serviços e portas
- Kibana: 5601
- Elasticsearch: 9200
- Logstash (Beats): 5044

## Documentação
- [Setup](docs/01-setup.md)
- [Dashboards no Kibana](docs/02-kibana-dashboards.md)
- [Troubleshooting](docs/03-troubleshooting.md)

