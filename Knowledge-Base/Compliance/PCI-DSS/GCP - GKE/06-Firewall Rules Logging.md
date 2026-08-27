# Firewall Rules Logging

## Objetivo

Para atender ao requisito **10.2.1 do PCI DSS v4.0.1**, deve ser habilitado o **Firewall Rules Logging**, permitindo registrar as conexões permitidas e negadas pelo firewall.

Esses registros auxiliam na detecção de tentativas de intrusão e complementam os **VPC Flow Logs** no monitoramento do tráfego de rede.

---

# Requisitos PCI-DSS relacionados

- PCI DSS 10.2.1

---

# O que é Firewall Rules Logging

O Firewall Rules Logging registra os eventos gerados pelas regras de firewall da VPC.

Sua finalidade é registrar:

- conexões permitidas;
- conexões negadas.

Esses registros fornecem informações adicionais sobre o comportamento da rede e apoiam a investigação de incidentes de segurança.

---

# Por que habilitar o Firewall Rules Logging

O Firewall Rules Logging como complemento aos VPC Flow Logs.

Enquanto:

- os **VPC Flow Logs** registram o tráfego da rede;

o **Firewall Rules Logging** registra:

- quais conexões foram permitidas;
- quais conexões foram bloqueadas.

Essas informações ajudam na detecção de tentativas de intrusão.

---

# Como habilitar o Firewall Rules Logging

O recurso deverá ser habilitado.

1. No menu de navegação, vá para **VPC Network** (Rede VPC) > **Firewall**.
2. Clique no **nome da regra de firewall** que você deseja monitorar.
3. Clique no botão **Edit** (Editar) no topo da página.
4. Role até a seção **Logs** e selecione **On** (Ativado).
5. (Opcional) Clique em **Show logs details** para configurar a amostragem ou se deseja incluir metadados.
6. Clique em **Save** (Salvar).

---

# Utilização em conjunto com os VPC Flow Logs

Utilizar ambos os recursos.

## VPC Flow Logs

Responsáveis pelo monitoramento do tráfego IP da rede.

---

## Firewall Rules Logging

Responsável pelo registro das decisões das regras de firewall.

Juntos, esses registros ampliam a visibilidade sobre os eventos da rede e apoiam a detecção de atividades suspeitas.

---

# Resultado esperado

Após a configuração:

- as conexões permitidas passam a ser registradas;
- as conexões negadas passam a ser registradas;
- os registros podem ser utilizados para investigação de eventos de segurança;
- o ambiente passa a contar com informações adicionais para detecção de tentativas de intrusão.

---

# Dependências

O Firewall Rules Logging será utilizado posteriormente por:

- [[Log Sinks]]
- [[BigQuery para Retenção e Análise]]
- [[Log Analytics]]
- [[Cloud Monitoring]]
- [[Security Command Center]]

---

# Evidências

Uma evidência possível, de acordo com o próprio contexto do documento, é demonstrar que o Firewall Rules Logging está habilitado nas regras de firewall utilizadas pelo ambiente.

---

# Observações

- o Firewall Rules Logging deve registrar conexões permitidas e negadas;
- sua finalidade é apoiar a detecção de tentativas de intrusão;
- esse recurso complementa os VPC Flow Logs no monitoramento da rede;
- o documento não apresenta instruções detalhadas de configuração nem comandos para habilitação do recurso.

---
Source: #source/internet_resources
Project: #project/compliance
Areas: #area/work
Subject: #subject/pci-dss
Type: #type/learning
Learning priority: #priority/P2
Status: #status/learning
Related: [[PCI-DSS-GCP-GKE]]
