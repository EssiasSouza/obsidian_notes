Source: #source/internet_resources
Project: #project/compliance
Areas: #area/work
Subject: #subject/pci-dss
Type: #type/learning
Learning priority: #priority/P2
Status: #status/learning
Related: [[PCI-DSS-GCP-GKE]]

---

# Data Access Logs

## Objetivo

O documento estabelece que, para atender ao requisito **10.2.1 do PCI DSS v4.0.1**, é obrigatório habilitar os **Data Access Logs** para os serviços que processam ou armazenam dados de portadores de cartão (CHD).

Enquanto os **Admin Activity Logs** registram operações administrativas, os **Data Access Logs** registram os acessos aos dados propriamente ditos, permitindo identificar quem leu ou modificou informações consideradas sensíveis.

---

# Requisitos PCI-DSS relacionados

- PCI DSS 10.2.1

---

# O que são Data Access Logs

Os Data Access Logs fazem parte dos **Cloud Audit Logs** e registram operações relacionadas ao acesso aos dados dos serviços do Google Cloud.

Segundo o documento, estes logs registram:

- Quem leu dados sensíveis.
- Quem modificou dados sensíveis.

O documento informa que esses logs normalmente permanecem desabilitados devido ao grande volume de registros gerados, porém são obrigatórios para o PCI DSS.

---

# Tipos de Data Access Logs

O documento apresenta dois tipos de logs que deverão ser habilitados.

## Data Read

Registra operações de leitura dos dados.

Exemplos descritos no documento:

- leitura de recursos;
- monitoramento de acesso a Secrets no Kubernetes.

---

## Data Write

Registra operações de gravação.

Inclui criação e atualização de recursos.

---

# Serviços mencionados no documento

O documento informa que os Data Access Logs deverão ser habilitados para os serviços que processam ou armazenam dados de cartões.

São citados como exemplo:

- Cloud Storage
- Cloud SQL
- BigQuery
- IAM

Para ambientes GKE também é citado:

- Kubernetes Engine API

---

# Como habilitar os Data Access Logs

## Passo 1

Abrir o Console do Google Cloud.

---

## Passo 2

Acessar:

```
IAM e administrador
    Logs de auditoria
```

---

## Passo 3

Selecionar o serviço desejado.

Exemplos apresentados no documento:

- Cloud Storage
- Cloud SQL
- BigQuery
- IAM

---

## Passo 4

No painel à direita localizar a seção:

```
Tipos de log
```

---

## Passo 5

Marcar:

- Data Read
- Data Write

---

## Passo 6

Salvar a configuração.

---

# Configuração para GKE

O documento apresenta um procedimento específico para clusters GKE Autopilot.

## Passo 1

Acessar:

```
IAM e administrador
    Logs de auditoria
```

---

## Passo 2

Pesquisar por:

```
Kubernetes Engine API
```

---

## Passo 3

Na seção **Tipos de log**, habilitar:

- Admin Read
- Data Read
- Data Write

---

## Passo 4

Salvar a configuração.

Segundo o documento, essa configuração é necessária para registrar quem acessou ou modificou recursos dentro do Kubernetes, como Secrets e ConfigMaps.

---

# Resultado esperado

Após a configuração:

- leituras de dados passam a ser registradas;
- gravações de dados passam a ser registradas;
- acessos aos recursos do Kubernetes passam a ser auditados;
- torna-se possível identificar quem acessou ou modificou informações sensíveis.

---

# Dependências

Os Data Access Logs serão utilizados posteriormente por:

- [[Log Sinks]]
- [[Cloud Storage]]
- [[BigQuery]]
- [[Log Router]]
- [[Log Analytics]]
- [[Security Command Center]]
- [[Cloud Monitoring]]

---

# Evidências sugeridas

Conforme o documento, podem ser obtidas capturas de tela mostrando:

- tela de Logs de Auditoria;
- Data Read habilitado;
- Data Write habilitado;
- serviços configurados;
- Kubernetes Engine API com Data Read e Data Write habilitados (quando houver GKE).

---

# Observações

Segundo o documento:

- os Data Access Logs normalmente ficam desabilitados devido ao volume de registros gerados;
- para atender ao PCI DSS, eles são obrigatórios;
- devem ser habilitados para todos os serviços que processam ou armazenam dados de portadores de cartão (CHD);
- no ambiente GKE, também devem ser habilitados para a Kubernetes Engine API a fim de registrar o acesso aos recursos do cluster.