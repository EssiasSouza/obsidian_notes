Source: #source/internet_resources
Project: #project/compliance
Areas: #area/work
Subject: #subect/pci-dss
Type: #type/learning
Learning priority: #priority/P2
Status: #status/lerning
Related: 

---

# PCI-DSS-GCP-GKE

# Inventário de produtos e configurações que deverão ser trabalhados

Organizei os itens por categoria para facilitar a criação dos tutoriais posteriormente.

| Categoria      | Produto / Configuração           | Onde aparece                     |
| -------------- | -------------------------------- | -------------------------------- |
| Logging        | Cloud Audit Logs                 | 10.2.1, 10.4.1, conclusão        |
| Logging        | Data Access Logs                 | 10.2.1, 10.4.1                   |
| Logging        | Admin Activity Logs              | 10.2.1                           |
| Logging        | Log Sinks                        | 10.2.1, 10.4.1, 10.4.2, 10.4.2.1 |
| Logging        | Log Router                       | Ambiente GKE                     |
| Logging        | Cloud Logging                    | diversos requisitos              |
| Logging        | Log Explorer                     | 10.4.3                           |
| Logging        | Log Analytics                    | 10.4.1, 10.4.1.1, 10.4.2, 10.4.3 |
| Logging        | Log-based Metrics                | 10.2.1                           |
| Logging        | Log-based Alerts                 | 10.4.1.1, 10.4.3                 |
| Rede           | VPC Flow Logs                    | 10.2.1, 10.4.1                   |
| Rede           | Firewall Rules Logging           | 10.2.1                           |
| Compute        | Compute Engine                   | 10.2.1                           |
| Compute        | Ops Agent                        | 10.2.1                           |
| Containers     | Kubernetes Audit Logs            | Ambiente GKE                     |
| Containers     | Kubernetes Engine API Audit Logs | Ambiente GKE                     |
| Containers     | GKE Autopilot                    | Ambiente GKE                     |
| Storage        | Cloud Storage                    | vários requisitos                |
| Banco de Dados | Cloud SQL                        | vários requisitos                |
| Banco de Dados | AlloyDB                          | 10.4.1                           |
| Analytics      | BigQuery                         | vários requisitos                |
| Analytics      | Scheduled Queries                | 10.4.1.1 e 10.4.2.1              |
| Monitoramento  | Cloud Monitoring                 | 10.2.1, 10.4.1, 10.4.2           |
| Segurança      | Security Command Center (SCC)    | 10.4.1, 10.4.1.1, 10.4.3         |
| Segurança      | Event Threat Detection           | 10.4.1, 10.4.1.1                 |
| Segurança      | Findings                         | 10.4.3                           |
| Segurança      | Access Transparency              | 10.2.1, 10.4.1                   |
| Segurança      | Cloud Armor                      | 10.4.1                           |
| Segurança      | Cloud IDS                        | 10.4.1                           |
| Segurança      | Identity Aware Proxy (IAP)       | 10.4.1                           |
| Segurança      | Cloud KMS                        | 10.4.1.1                         |
| SIEM           | Chronicle SIEM                   | 10.4.1.1                         |
| Aplicações     | Cloud Run                        | vários requisitos                |
| Aplicações     | Cloud Functions                  | vários requisitos                |
| Aplicações     | Error Reporting                  | 10.4.3                           |
| Integração     | Pub/Sub                          | 10.4.2 e 10.4.2.1                |
| Integração     | Dataflow                         | 10.4.2                           |
| Automação      | Cloud Scheduler                  | 10.4.2                           |
| DevOps         | Cloud Build                      | 10.4.2 e 10.4.2.1                |
| DevOps         | Artifact Registry                | 10.4.2.1                         |
| Eventos        | Eventarc                         | 10.4.3                           |
| Identidade     | IAM                              | diversos requisitos              |
| Processos      | Targeted Risk Analysis (TRA)     | 10.4.2 e 10.4.2.1                |
| Processos      | Sistema de Gestão de Incidentes  | 10.4.3                           |
| Processos      | ServiceNow                       | 10.4.2.1                         |

# Configurações específicas que o documento solicita executar

Além dos produtos, o documento descreve configurações específicas que deverão ser implementadas.

## Cloud Audit Logs

- habilitar Data Read
- habilitar Data Write
- selecionar os serviços que armazenam ou processam CHD
- verificar Admin Activity

## Log Sinks

- criar coletores
- exportar para Cloud Storage
- exportar para BigQuery
- configurar retenção de um ano
- definir filtros

## VPC

- habilitar VPC Flow Logs
- habilitar Firewall Rules Logging

## Compute Engine

- instalar Ops Agent
- configurar coleta de Syslog
- configurar Event Log (Windows)

## GKE

- habilitar Kubernetes API Audit Logs
- habilitar Admin Read
- habilitar Data Read
- habilitar Data Write
- validar retenção via Log Router
- validar VPC Flow Logs da subnet

## Cloud Monitoring

- criar dashboards
- criar alertas
- criar métricas baseadas em logs

## Cloud Logging

- criar consultas
- criar consultas salvas
- configurar Log Analytics

## BigQuery

- criar datasets para logs
- criar Scheduled Queries
- gerar relatórios

## Security Command Center

- habilitar SCC
- habilitar Event Threat Detection
- acompanhar Findings
- alterar status para Resolved
- alterar status para Muted

## Processos

- documentar revisão diária
- documentar revisão periódica
- criar TRA
- registrar evidências
- registrar tratamento das anomalias
- integrar alertas ao sistema de chamados

# Ordem sugerida para criação dos tutoriais

Essa ordem segue exatamente a dependência lógica apresentada pelo documento.

1. Cloud Audit Logs (Admin logs e Data Access Logs)
2. 
3. Log Sinks
4. Cloud Storage para retenção
5. BigQuery para retenção e análise
6. VPC Flow Logs
7. Firewall Rules Logging
8. Ops Agent
9. Kubernetes Audit Logs
10. Log Router
11. Cloud Monitoring
12. Log-based Metrics
13. Log-based Alerts
14. Log Analytics
15. Scheduled Queries no BigQuery
16. Dashboards
17. Security Command Center
18. Event Threat Detection
19. Access Transparency
20. Chronicle SIEM (quando aplicável)
21. Error Reporting
22. Eventarc
23. Processo de revisão diária
24. Processo de revisão periódica
25. Targeted Risk Analysis (TRA)
26. Processo de tratamento de anomalias
27. Produção de evidências para auditoria