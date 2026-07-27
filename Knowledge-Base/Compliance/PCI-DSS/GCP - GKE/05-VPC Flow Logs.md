Source: #source/internet_resources
Project: #project/compliance
Areas: #area/work
Subject: #subject/pci-dss
Type: #type/learning
Learning priority: #priority/P2
Status: #status/learning
Related: [[PCI-DSS-GCP-GKE]]

---
# VPC Flow Logs

## Objetivo

Para atender ao requisito **10.2.1 do PCI DSS v4.0.1**, é necessário habilitar os **VPC Flow Logs** nas sub-redes que fazem parte do ambiente de dados de portadores de cartões (CDE).

Os VPC Flow Logs são utilizados para monitorar o tráfego de rede, apoiar a detecção de anomalias e fornecer evidências para investigações de segurança.

---

# Requisitos PCI-DSS relacionados

- PCI DSS 10.2.1
- PCI DSS 10.4.1

---

# O que são VPC Flow Logs

Os VPC Flow Logs registram informações sobre o tráfego de rede nas sub-redes da VPC.

Sua finalidade é:

- detectar anomalias;
- identificar atividades suspeitas;
- monitorar o tráfego de rede;
- apoiar investigações de segurança.

---

# Por que habilitar os VPC Flow Logs

A segunda parte do requisito **10.2.1** exige mecanismos para detectar anomalias e atividades suspeitas na rede.

Para atender a esse requisito, deverão ser habilitados os logs de fluxo de rede das sub-redes pertencentes ao ambiente CDE.

---

# Como habilitar os VPC Flow Logs

## Passo 1

Abrir o Console do Google Cloud.

---

## Passo 2

Acessar:

```text
VPC Network
    Subnets
```

---

## Passo 3

Selecionar a sub-rede pertencente ao ambiente de dados de portadores de cartões (CDE).

---

## Passo 4

Habilitar:

```text
VPC Flow Logs
```

---

## Passo 5

Salvar a configuração.

---

# Verificação da configuração

Também é possível verificar se os VPC Flow Logs estão habilitados utilizando o comando:

```bash
gcloud compute networks subnets describe prd-public-1 \
    --region=southamerica-east1 \
    --format="get(logConfig)"
```

---

# Configuração para ambiente GKE

Para clusters GKE Autopilot:

- a infraestrutura dos nós é gerenciada pela Google;
- a responsabilidade pelo monitoramento da rede permanece sendo da organização.

Assim, deve-se:

1. acessar **VPC Network > Subnets**;
2. localizar a sub-rede utilizada pelo cluster;
3. verificar se a opção **VPC Flow Logs** está habilitada.

Essa configuração registra o tráfego IP entre os pods e também o tráfego para fora do cluster.

---

# Relação com Firewall Rules Logging

VPC Flow Logs juntamente com o **Firewall Rules Logging** como mecanismos de monitoramento da rede.

Enquanto: **VPC Flow Logs** registram o tráfego de rede o **Firewall Rules Logging** registra conexões permitidas e negadas.

Esses dois recursos serão utilizados em conjunto para apoiar a detecção de anomalias.

---

# Resultado esperado

Após a configuração:

- o tráfego de rede das sub-redes do CDE passa a ser registrado;
- torna-se possível acompanhar o tráfego IP relacionado ao ambiente;
- os registros podem ser utilizados durante investigações de segurança;
- o ambiente passa a oferecer suporte à detecção de atividades suspeitas na rede.

---

# Dependências

Os VPC Flow Logs serão utilizados posteriormente por:

- [[Log Sinks]]
- [[BigQuery para Retenção e Análise]]
- [[Security Command Center]]
- [[Log Analytics]]
- [[Cloud Monitoring]]

---

# Evidências

Podem ser obtidas capturas de tela mostrando:

- lista de sub-redes;
- opção **VPC Flow Logs** habilitada;
- configuração da sub-rede utilizada pelo ambiente CDE;
- saída do comando de verificação da configuração.

---

# Observações

- os VPC Flow Logs devem ser habilitados em cada sub-rede pertencente ao ambiente CDE;
- no ambiente GKE Autopilot, a configuração deve ser validada na sub-rede utilizada pelo cluster;
- os VPC Flow Logs apoiam a detecção de anomalias exigida pelo PCI DSS;
- juntamente com o Firewall Rules Logging, fornecem visibilidade sobre o tráfego de rede e auxiliam na identificação de atividades suspeitas.