# BigQuery para Retenção e Análise

## Objetivo

O **BigQuery** pode ser utilizado como destino dos **Log Sinks** para centralizar os logs de auditoria, permitindo consultas SQL, análises forenses, geração de relatórios e execução de consultas agendadas.

Ao longo dos requisitos da família 10 do PCI DSS, o BigQuery é apresentado como a principal plataforma para análise dos eventos registrados no ambiente Google Cloud.

---

# Requisitos PCI-DSS relacionados

- PCI DSS 10.2.1
- PCI DSS 10.4.1
- PCI DSS 10.4.1.1
- PCI DSS 10.4.2
- PCI DSS 10.4.2.1

---

# Finalidade do BigQuery

O BigQuery pode ser utilizado para:

- armazenar logs exportados pelos Log Sinks;
- realizar análises forenses;
- executar consultas SQL;
- gerar relatórios;
- executar consultas agendadas;
- produzir evidências para auditoria.

---

# BigQuery no fluxo de auditoria

Fluxo de utilização:

```text
Cloud Audit Logs
        ↓
     Log Sink
        ↓
     BigQuery
        ↓
Consultas SQL
        ↓
Relatórios
        ↓
Evidências de Auditoria
```

---

# Como utilizar o BigQuery

## Passo 1

Criar um **Log Sink** no Cloud Logging.

---

## Passo 2

Selecionar o **BigQuery** como destino.

---

## Passo 3

Concluir a criação do coletor.

Os logs exportados passarão a ser armazenados no BigQuery.

---

# Utilizações

## Análise forense

O BigQuery pode ser utilizado para análises forenses rápidas sobre os logs exportados.

---

## Consultas SQL

Utilizar consultas SQL para identificar padrões suspeitos.

Exemplos apresentados:

- excesso de erros 403;
- autenticações incomuns;
- comportamentos fora do padrão esperado.

---

## Scheduled Queries

Recomenda-se criar consultas agendadas no BigQuery para execução automática.

Como exemplos, cita:

- execução a cada hora;
- execução diária;
- execução semanal, conforme a frequência definida.

---

## Relatórios

Gerar relatórios contendo:

- eventos classificados como erro;
- avisos relevantes;
- acessos incomuns;
- comportamentos que mereçam investigação;
- possíveis anomalias.

Esses relatórios servirão para facilitar a revisão e produzir evidências do processo.

---

# Componentes

BigQuery para análise de eventos relacionados a:

- Cloud Audit Logs;
- Cloud Storage;
- BigQuery;
- IAM;
- Cloud KMS;
- componentes do CDE;
- componentes de suporte.

---

# Integração com outros requisitos

## Requisito 10.4.1

Criar consultas diárias resumindo os eventos de segurança das últimas 24 horas.

---

## Requisito 10.4.1.1

Criar Scheduled Queries para executar análises periódicas.

O documento cita exemplos como:

- usuários acessando dados sensíveis fora do horário esperado;
- aumento repentino de erros de autorização (403);
- alterações inesperadas em permissões do IAM;
- alterações em chaves do Cloud KMS.

---

## Requisito 10.4.2

Gerar relatórios semanais contendo:

- erros;
- avisos;
- acessos incomuns;
- eventos relevantes.

---

## Requisito 10.4.2.1

Executar Scheduled Queries na frequência definida pela Targeted Risk Analysis (TRA).

---

# Resultado esperado

Após a configuração:

- os logs exportados passam a ser armazenados no BigQuery;
- consultas SQL podem ser executadas sobre os registros;
- relatórios podem ser produzidos automaticamente;
- o ambiente passa a oferecer suporte à investigação e à produção de evidências para auditoria.

---

# Dependências

O BigQuery depende de:

- [[Cloud Audit Logs]]
- [[Data Access Logs]]
- [[Log Sinks]]

Será utilizado posteriormente por:

- [[Log Analytics]]
- [[Scheduled Queries]]
- [[Cloud Monitoring]]
- [[Security Command Center]]
- [[Targeted Risk Analysis (TRA)]]

---

# Evidências

Podem ser obtidas capturas de tela mostrando:

- Log Sink utilizando BigQuery como destino;
- dataset criado para armazenamento dos logs;
- consultas SQL executadas;
- Scheduled Queries configuradas;
- relatórios gerados.

---

# Observações

- o BigQuery pode ser utilizado como destino dos Log Sinks;
- permite realizar análises forenses rápidas sobre os logs;
- possibilita a criação de consultas SQL para identificação de padrões suspeitos;
- suporta Scheduled Queries para automatizar análises periódicas;
- os relatórios produzidos servem como evidências da revisão prevista pelos requisitos da família 10 do PCI DSS.

---
Source: #source/internet_resources
Project: #project/compliance
Areas: #area/work
Subject: #subject/pci-dss
Type: #type/learning
Learning priority: #priority/P2
Status: #status/learning
Related: [[PCI-DSS-GCP-GKE]]
