# Cloud Storage para Retenção

## Objetivo

O documento estabelece que o **Cloud Storage** deve ser utilizado como repositório de longo prazo para armazenar os logs exportados pelo **Cloud Logging** por meio dos **Log Sinks**, permitindo atender ao período de retenção exigido pelo PCI DSS.

No ambiente GKE, o documento também orienta configurar um bucket com política de retenção de **365 dias**.

---

# Requisitos PCI-DSS relacionados

- PCI DSS 10.2.1
- PCI DSS 10.4.1
- PCI DSS 10.4.2
- PCI DSS 10.4.2.1

---

# Finalidade do Cloud Storage

O Cloud Storage será utilizado para:

- arquivamento de longo prazo dos logs de auditoria;
- atender ao período mínimo de retenção exigido pelo PCI DSS;
- servir como destino dos Log Sinks.

Diferença do Cloud Storage do BigQuery:

| Serviço | Finalidade |
|----------|------------|
| Cloud Storage | Arquivamento de longo prazo |
| BigQuery | Análise forense e consultas |

---

# Por que utilizar o Cloud Storage

O documento informa que:

- os logs de **Admin Activity** permanecem armazenados por aproximadamente **400 dias**;
- os logs de **Data Access** permanecem armazenados por apenas **30 dias** por padrão.

Como o PCI DSS exige retenção superior ao período padrão dos Data Access Logs, os registros deverão ser exportados para um armazenamento de longo prazo utilizando um Log Sink.

---

# Como utilizar o Cloud Storage

## Passo 1

Criar um bucket do Cloud Storage.

---

## Passo 2

Criar um **Log Sink** no Cloud Logging.

---

## Passo 3

Selecionar o bucket criado como destino do coletor.

---

## Passo 4

Configurar o filtro de exportação.

O documento apresenta como filtro recomendado:

```text
logName:"cloudaudit.googleapis.com"
```

---

## Passo 5

Concluir a criação do Log Sink.

---

# Configuração para ambiente GKE

Para clusters GKE Autopilot, o documento orienta:

- criar um Log Sink;
- enviar os logs do cluster para um bucket do Cloud Storage;
- configurar uma política de retenção de **365 dias**.

Também apresenta o seguinte filtro para capturar os logs do cluster:

```text
resource.type="k8s_cluster"
resource.labels.cluster_name="gke-cluster-prd"
logName:"cloudaudit.googleapis.com"
```

---

# Utilização

O Cloud Storage é repositório para:

- Cloud Audit Logs;
- Data Access Logs;
- logs do GKE;
- retenção de longo prazo.

---

# Resultado esperado

Após a configuração:

- os logs exportados pelos Log Sinks passam a ser armazenados no Cloud Storage;
- os registros permanecem disponíveis para retenção de longo prazo;
- os logs deixam de depender exclusivamente da retenção padrão do Cloud Logging.

---

# Dependências

O Cloud Storage depende de:

- [[Cloud Audit Logs]]
- [[Data Access Logs]]
- [[Log Sinks]]

Será utilizado posteriormente por:

- [[Log Analytics]]
- [[Cloud Monitoring]]
- [[Security Command Center]]

---

# Evidências

Podem ser obtidas capturas de tela mostrando:

- bucket utilizado para retenção;
- política de retenção configurada (quando aplicável);
- Log Sink apontando para o bucket;
- filtro configurado no coletor.

---

# Observações

- o Cloud Storage deve ser utilizado para arquivamento de longo prazo dos logs;
- os Log Sinks deverão exportar os logs de auditoria para esse bucket;
- no ambiente GKE, o bucket deve possuir política de retenção de **365 dias**;
- o Cloud Storage é utilizado como mecanismo para atender aos requisitos de retenção dos registros previstos na família de requisitos 10 do PCI DSS.

---
Source: #source/internet_resources
Project: #project/compliance
Areas: #area/work
Subject: #subject/pci-dss
Type: #type/learning
Learning priority: #priority/P2
Status: #status/learning
Related: [[PCI-DSS-GCP-GKE]]
