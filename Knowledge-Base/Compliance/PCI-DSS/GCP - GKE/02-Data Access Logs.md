Source: #source/internet_resources
Project: #project/compliance
Areas: #area/work
Subject: #subject/pci-dss
Type: #type/learning
Learning priority: #priority/P2
Status: #status/learning
Related: [[PCI-DSS-GCP-GKE]]

---

# Log Sinks

## Objetivo

O documento estabelece que, para atender aos requisitos da família **10 do PCI DSS v4.0.1**, é necessário configurar **Log Sinks** para exportar os logs de auditoria do Cloud Logging para um repositório de longo prazo.

Os Log Sinks serão responsáveis por centralizar os logs produzidos pelos diversos serviços do Google Cloud, permitindo atender aos requisitos de retenção, investigação forense, revisão de eventos e produção de evidências de auditoria.

---

# Requisitos PCI-DSS relacionados

- PCI DSS 10.2.1
- PCI DSS 10.4.1
- PCI DSS 10.4.2
- PCI DSS 10.4.2.1

---

# O que são Log Sinks

Um **Log Sink** (Coletor de Registros) é um recurso do Cloud Logging utilizado para exportar logs para outro destino.

Segundo o documento, os Log Sinks deverão ser utilizados para enviar os logs de auditoria para:

- Cloud Storage, destinado ao arquivamento de longo prazo.
- BigQuery, destinado à análise forense e consultas.

---

# Por que utilizar Log Sinks

O documento informa que:

- os logs de **Admin Activity** permanecem armazenados por aproximadamente **400 dias**;
- os logs de **Data Access** permanecem armazenados por apenas **30 dias** por padrão.

Como o PCI DSS exige retenção mínima de um ano, torna-se necessário exportar esses logs para outro repositório utilizando Log Sinks.

---

# Destinos citados no documento

## Cloud Storage

Utilizado para arquivamento de longo prazo.

No ambiente GKE, o documento recomenda configurar um bucket com política de retenção de **365 dias**.

---

## BigQuery

Utilizado para:

- consultas SQL;
- análise forense;
- geração de relatórios;
- consultas agendadas (Scheduled Queries).

---

# Como criar um Log Sink

## Passo 1

Abrir o Console do Google Cloud.

---

## Passo 2

Acessar:

```
Logging
    Roteador de registros
```

---

## Passo 3

Selecionar:

```
Criar coletor
```

---

## Passo 4

Escolher o destino.

Segundo o documento, os destinos poderão ser:

- Cloud Storage
- BigQuery

---

## Passo 5

Definir o filtro.

O documento apresenta o seguinte filtro recomendado para os logs de auditoria:

```text
logName:"cloudaudit.googleapis.com"
```

---

## Passo 6

Criar o coletor.

---

# Configuração para ambiente GKE

Para clusters GKE Autopilot, o documento apresenta um filtro específico.

```text
resource.type="k8s_cluster"
resource.labels.cluster_name="gke-cluster-prd"
logName:"cloudaudit.googleapis.com"
```

Segundo o documento, esse filtro captura os logs de auditoria do cluster.

---

# Utilizações dos Log Sinks ao longo do documento

Os Log Sinks são utilizados para:

- exportar Cloud Audit Logs;
- garantir retenção mínima de um ano;
- centralizar os logs em Cloud Storage ou BigQuery;
- fornecer dados para consultas no Log Analytics;
- alimentar consultas agendadas no BigQuery;
- gerar relatórios periódicos;
- produzir evidências para auditorias.

---

# Resultado esperado

Após a configuração:

- os logs deixam de depender apenas da retenção padrão do Cloud Logging;
- os logs ficam armazenados em um repositório centralizado;
- torna-se possível realizar consultas e investigações utilizando BigQuery;
- torna-se possível atender ao período de retenção exigido pelo PCI DSS.

---

# Dependências

Os Log Sinks serão utilizados posteriormente por:

- [[Cloud Storage]]
- [[BigQuery]]
- [[Log Analytics]]
- [[Scheduled Queries]]
- [[Cloud Monitoring]]
- [[Security Command Center]]

---

# Evidências sugeridas

Conforme o documento, podem ser obtidas capturas de tela mostrando:

- tela do Roteador de Registros;
- lista de Log Sinks configurados;
- destino configurado;
- filtro utilizado;
- bucket do Cloud Storage (quando utilizado);
- dataset do BigQuery (quando utilizado).

---

# Observações

Segundo o documento:

- os Log Sinks deverão exportar todos os logs de auditoria;
- Cloud Storage deve ser utilizado para arquivamento de longo prazo;
- BigQuery pode ser utilizado para análises forenses e consultas rápidas;
- no ambiente GKE existe um filtro específico para exportação dos logs do cluster;
- os Log Sinks são utilizados em diversos requisitos da família 10 do PCI DSS como mecanismo de centralização e retenção dos registros de auditoria.