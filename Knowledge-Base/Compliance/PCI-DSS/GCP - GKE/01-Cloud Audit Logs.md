Source: #source/internet_resources
Project: #project/compliance
Areas: #area/work
Subject: #subject/pci-dss
Type: #type/learning
Learning priority: #priority/P2
Status: #status/learning
Related: [[PCI-DSS-GCP-GKE]]

---

# Cloud Audit Logs

## Objetivo

Para atender ao requisito **10.2.1 do PCI DSS v4.0.1**, é necessário garantir que os **Cloud Audit Logs** estejam habilitados para todos os componentes do sistema que processam ou armazenam dados de portadores de cartão (CHD).

Os Cloud Audit Logs serão a principal fonte de auditoria do ambiente GCP e servirão como base para os demais requisitos relacionados ao monitoramento, revisão de eventos e geração de evidências.

---

# Requisitos PCI-DSS relacionados

- PCI DSS 10.2.1
- PCI DSS 10.4.1
- PCI DSS 10.4.1.1
- PCI DSS 10.4.2
- PCI DSS 10.4.3

---

# O que são Cloud Audit Logs

Cloud Audit Logs registram operações executadas nos serviços do Google Cloud, permitindo identificar:

- Quem executou uma ação.
- Qual recurso foi acessado.
- Quando a ação ocorreu.
- Qual operação foi realizada.
- Se a operação foi concluída com sucesso ou falhou.

Os Cloud Audit Logs constituem o principal mecanismo de auditoria utilizado para atender aos requisitos do PCI DSS no ambiente Google Cloud.

---

# Tipos de logs

Existem os seguintes tipos de logs.

## Admin Activity

Registra operações administrativas realizadas nos serviços do Google Cloud.

Segundo o documento, este tipo de log já vem habilitado por padrão.

---

## Data Access

Registra acessos aos dados.

Segundo o documento, estes logs normalmente ficam desabilitados devido ao grande volume gerado, porém são obrigatórios para o PCI DSS.

Os seguintes tipos deverão ser habilitados:

- Data Read
- Data Write

---

# Serviços mencionados no documento

O documento informa que os serviços que processam ou armazenam dados de cartões devem possuir Data Access Logs habilitados.

Como exemplos, são citados:

- Cloud Storage
- Cloud SQL
- BigQuery
- IAM

No ambiente GKE, também é citado:

- Kubernetes Engine API

---

# Como habilitar os Cloud Audit Logs

## Passo 1

Acessar:

```
IAM e administrador
    Logs de auditoria
```

---

## Passo 2

Selecionar o serviço desejado.

Exemplos apresentados no documento:

- Cloud Storage
- Cloud SQL
- BigQuery
- IAM

---

## Passo 3

No painel à direita localizar **Tipos de log**.

---

## Passo 4

Habilitar:

- Data Read
- Data Write

OBS.: Admin read já é ativado por padrão.

---

## Passo 5

Salvar a configuração.

---

# Ambiente GKE

Para clusters GKE Autopilot, o documento apresenta um procedimento específico.

## Caminho

```
IAM e administrador
    Logs de auditoria
```

Pesquisar por:

```
Kubernetes Engine API
```

Em **Tipos de log**, habilitar:

- Admin Read
- Data Read
- Data Write

Salvar a configuração.

---

# Resultado esperado

Após a configuração:

- operações administrativas continuarão sendo registradas;
- acessos aos dados serão registrados;
- modificações em dados serão registradas;
- recursos do Kubernetes também terão seus acessos registrados.

---

# Dependências

Os Cloud Audit Logs serão utilizados posteriormente por:

- [[Log Sinks]]
- [[Log Router]]
- [[Log Analytics]]
- [[Cloud Monitoring]]
- [[Security Command Center]]
- [[Log-based Metrics]]
- [[Log-based Alerts]]

---

# Evidências

As evidências podem ser obtidas por meio de capturas de tela mostrando:

- tela de Logs de Auditoria;
- serviços configurados;
- Data Read habilitado;
- Data Write habilitado;
- Kubernetes Engine API configurada (quando houver GKE).

---
