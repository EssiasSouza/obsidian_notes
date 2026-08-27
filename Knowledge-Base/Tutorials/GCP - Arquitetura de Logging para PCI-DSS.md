# GCP: Arquitetura de Logging para PCI-DSS

## Objetivo

Este procedimento descreve como configurar uma arquitetura de logging no Google Cloud para manter eventos de auditoria relevantes em duas camadas:

1. **Cloud Logging**, para investigação e consulta operacional.
2. **Cloud Storage**, para retenção de longo prazo com armazenamento otimizado por meio do Autoclass.

A arquitetura proposta utiliza um Log Bucket dedicado chamado `pci-audit` e um bucket do Cloud Storage chamado `pci10-admin-activity`.

O conjunto de logs será controlado por um filtro de Log Router e encaminhado para os dois destinos.

```text
                           Cloud Logging
                                │
                           Log Router
                                │
                  ┌─────────────┴─────────────┐
                  │                           │
                  ▼                           ▼
        Sink: pci10-admin-activity-logging   Sink: pci10-admin-activity-gcs
                  │                           │
                  ▼                           ▼
             Log Bucket                  Cloud Storage
             pci-audit                 pci10-admin-activity
                  │                           │
                  ▼                           ▼
            Logs Explorer                Autoclass
            / Analytics                     │
                                            ▼
                                    Long-term storage
```

> **Importante:** o Log Bucket e o bucket do Cloud Storage são recursos diferentes. O primeiro pertence ao Cloud Logging; o segundo pertence ao Cloud Storage.

---

# 1. Pré-requisitos

Antes de iniciar, confirme que:

* o projeto GCP está definido;
* o Cloud Logging está habilitado;
* o usuário que executará as alterações possui as permissões necessárias;
* existe uma definição organizacional sobre quais logs fazem parte do escopo PCI-DSS;
* existe uma política de retenção aprovada para os logs;
* a região do armazenamento foi definida de acordo com os requisitos da organização.

Para operações em produção, recomenda-se realizar as alterações por meio do processo de Change Management utilizado pela organização.

---

# 2. Identificar os Log Buckets existentes

Comece verificando os Log Buckets atualmente configurados no projeto.

```text
gcloud logging buckets list --project=PROJECT_ID
```

A configuração padrão normalmente inclui:

```text
_Required
_Default
```

Esses recursos são **Log Buckets do Cloud Logging**, e não buckets do Cloud Storage.

A arquitetura existente pode ser representada assim:

```text
Cloud Logging
    │
    ├── _Required
    │
    └── _Default
```

---

# 3. Verificar exclusions existentes

Antes de criar o novo mecanismo de roteamento, verifique as exclusions existentes no projeto.

Isso é particularmente importante quando o `_Default` possui filtros destinados a reduzir o volume de logs armazenados.

Uma configuração como:

```text
severity < ERROR
```

pode fazer com que determinados logs deixem de ser armazenados naquele destino.

Não se deve concluir, apenas pela ausência de um log no `_Default`, que o evento não está sendo produzido pelo serviço.

O objetivo desta etapa é identificar se existem regras de exclusão que possam interferir na estratégia de logging.

---

# 4. Criar o Log Bucket `pci-audit`

Crie um Log Bucket dedicado para os eventos de auditoria que precisam permanecer consultáveis no Cloud Logging.

O recurso terá o seguinte identificador:

```text
projects/PROJECT_ID/locations/global/buckets/pci-audit
```

A arquitetura ficará:

```text
Cloud Logging
    │
    ├── _Required
    ├── _Default
    │
    └── pci-audit
```

O bucket `pci-audit` deve possuir uma política de retenção compatível com os requisitos definidos pela organização.

### Por que criar um bucket separado?

A separação permite aplicar uma política específica aos logs de auditoria, sem depender da configuração do `_Default`.

Também facilita:

* investigação;
* controle de acesso;
* definição de retenção;
* documentação;
* auditoria da configuração.

---

# 5. Definir o filtro dos logs

O filtro é uma das partes mais importantes da configuração.

Ele determina quais Log Entries serão encaminhados pelos sinks.

Para este exemplo, serão utilizados os logs de **Admin Activity** do Cloud Audit Logs:

```text
logName:"cloudaudit.googleapis.com%2Factivity"
```

O filtro deve ser adaptado ao escopo real do ambiente.

Por exemplo, caso seja necessário restringir ainda mais os eventos, podem ser adicionados critérios relacionados a:

* serviço;
* método;
* recurso;
* principal;
* tipo de recurso;
* severidade.

Não é recomendado copiar todos os logs indiscriminadamente para o armazenamento de longo prazo sem antes avaliar volume, custo e necessidade de retenção.

---

# 6. Criar o bucket do Cloud Storage

Crie um bucket do Cloud Storage com o nome:

```text
pci10-admin-activity
```

O endereço será:

```text
gs://pci10-admin-activity
```

Esse bucket será utilizado como camada de armazenamento de longo prazo.

A separação fica:

```text
Cloud Logging
     │
     ▼
  pci-audit
     │
     └── investigação

Cloud Storage
     │
     ▼
pci10-admin-activity
     │
     └── retenção de longo prazo
```

A localização do bucket deve ser escolhida de acordo com os requisitos de segurança, residência de dados e governança da organização.

---

# 7. Configurar Autoclass no Cloud Storage

O Cloud Storage possui o recurso **Autoclass**, que permite ajustar automaticamente a classe de armazenamento dos objetos com base no padrão de acesso.

Isso evita a necessidade de criar uma rotina própria para mover objetos entre classes.

A arquitetura passa a ser:

```text
gs://pci10-admin-activity
             │
          Autoclass
             │
      ┌──────┴──────┐
      │             │
      ▼             ▼
  acesso        sem acesso
      │             │
      ▼             ▼
  classe        classe
  adequada      mais fria
```

O Autoclass deve ser tratado como mecanismo de **otimização de armazenamento**, e não como mecanismo de retenção.

São conceitos diferentes:

```text
Retention Policy
    └── determina por quanto tempo o objeto pode existir

Autoclass
    └── determina a classe de armazenamento apropriada
```

---

# 8. Criar o Sink para o Log Bucket

Como um Log Sink possui um único destino, será necessário criar um sink específico para o Log Bucket `pci-audit`.

Nome:

```text
pci10-admin-activity-logging
```

Filtro:

```text
logName:"cloudaudit.googleapis.com%2Factivity"
```

Destino:

```text
projects/PROJECT_ID/locations/global/buckets/pci-audit
```

O fluxo será:

```text
Cloud Logging
      │
      ▼
Log Router
      │
      ▼
pci10-admin-activity-logging
      │
      ▼
pci-audit
```

---

# 9. Criar o Sink para o Cloud Storage

Crie um segundo sink utilizando o mesmo filtro.

Nome:

```text
pci10-admin-activity-gcs
```

Filtro:

```text
logName:"cloudaudit.googleapis.com%2Factivity"
```

Destino:

```text
gs://pci10-admin-activity
```

O fluxo será:

```text
Cloud Logging
      │
      ▼
Log Router
      │
      ▼
pci10-admin-activity-gcs
      │
      ▼
gs://pci10-admin-activity
```

O resultado final será:

```text
                         Log Entry
                             │
                       Cloud Logging
                             │
                        Log Router
                             │
                 ┌───────────┴───────────┐
                 │                       │
                 ▼                       ▼
       pci10-admin-activity-      pci10-admin-activity-
             logging                     gcs
                 │                       │
                 ▼                       ▼
             pci-audit             GCS Bucket
```

Os dois sinks utilizam o mesmo filtro, mas possuem destinos independentes.

---

# 10. Configurar as permissões dos sinks

Os sinks que encaminham logs para recursos externos precisam possuir autorização para gravar no destino.

O procedimento deve considerar a **service account associada ao sink** e conceder somente as permissões necessárias ao destino.

Para o bucket GCS, a identidade do sink deve possuir a permissão necessária para criar objetos no bucket.

Para o Log Bucket, devem ser concedidas as permissões necessárias para que o Log Router possa encaminhar os eventos.

A recomendação é utilizar **least privilege** e evitar conceder papéis amplos no projeto apenas para fazer o roteamento funcionar.

---

# 11. Validar o Log Bucket

Depois de criar os sinks, gere ou aguarde um evento de Admin Activity e consulte o bucket `pci-audit`.

No Logs Explorer, selecione o Log Bucket:

```text
pci-audit
```

E utilize o filtro:

```text
logName:"cloudaudit.googleapis.com%2Factivity"
```

O objetivo é confirmar que o evento chegou ao bucket dedicado.

---

# 12. Validar o Cloud Storage

Depois da mesma operação, verifique o bucket:

```text
gs://pci10-admin-activity
```

Os objetos encaminhados pelo Cloud Logging serão armazenados no Cloud Storage em uma estrutura de diretórios gerenciada pelo serviço.

O objetivo desta etapa não é verificar se os objetos possuem exatamente um determinado nome ou caminho, mas confirmar que o evento foi efetivamente exportado para o bucket.

---

# 13. Validar os dois destinos

A validação final deve confirmar:

```text
                    Admin Activity
                          │
                          ▼
                    Cloud Logging
                          │
                    Log Router
                          │
             ┌────────────┴────────────┐
             ▼                         ▼
        pci-audit              pci10-admin-activity
             │                         │
             ▼                         ▼
       Logs Explorer                  GCS
```

O mesmo evento deve estar disponível nos dois destinos.

Essa validação é especialmente importante antes de considerar a mudança concluída.

---

# 14. Definir a retenção do Log Bucket

O `pci-audit` deve ser configurado com a retenção definida pela política de segurança da organização.

Por exemplo:

```text
pci-audit
    │
    └── retenção operacional
```

A ideia é manter no Cloud Logging somente o período necessário para investigação operacional e auditoria frequente.

O armazenamento de longo prazo ficará no GCS.

Isso evita utilizar o Log Bucket como arquivo histórico indefinidamente.

---

# 15. Definir a retenção do GCS

A retenção no GCS deve ser definida separadamente.

Por exemplo:

```text
Cloud Logging
     │
     └── retenção operacional

Cloud Storage
     │
     └── retenção de longo prazo
```

O período deve ser definido com base na política de retenção da organização e nos requisitos aplicáveis ao ambiente PCI-DSS.

Não confunda:

**90 dias de retenção no Cloud Logging**

com:

**90 dias de retenção total dos logs.**

Se o GCS estiver configurado para preservar os objetos por um período maior, o log continuará disponível no armazenamento histórico mesmo depois de expirar do Log Bucket.

---

# 16. Investigação de logs antigos

Depois que um evento deixar de existir no Log Bucket devido à política de retenção, ele não estará mais disponível para consulta normal no Logs Explorer daquele bucket.

O histórico continuará no GCS:

```text
Cloud Logging
     │
     └── pci-audit
            │
            └── expira após retenção

Cloud Storage
     │
     └── pci10-admin-activity
            │
            └── mantém histórico
```

Nesse cenário, o processo de investigação de um evento antigo deve considerar ferramentas apropriadas para consultar ou processar os objetos armazenados no Cloud Storage.

Portanto, é importante documentar o procedimento de recuperação antes de colocar a arquitetura em produção.

---

# 17. Considerações de segurança

Para uma implementação voltada a PCI-DSS, o bucket GCS não deve ser tratado simplesmente como um diretório para arquivos.

Avalie, conforme a política da organização:

* IAM;
* princípio do menor privilégio;
* criptografia;
* CMEK, quando aplicável;
* retenção;
* proteção contra exclusão;
* versionamento, quando aplicável;
* auditoria de acesso;
* residência dos dados;
* procedimento de recuperação;
* monitoramento das configurações.

O mesmo princípio vale para o Log Bucket `pci-audit`.

---

# 18. Arquitetura final

Após todas as configurações, a arquitetura será:

```text
                              GCP
                               │
                               ▼
                        Cloud Audit Logs
                               │
                               ▼
                        Cloud Logging
                               │
                         Log Router
                               │
             ┌─────────────────┴─────────────────┐
             │                                   │
             ▼                                   ▼
pci10-admin-activity-logging        pci10-admin-activity-gcs
             │                                   │
             ▼                                   ▼
        Log Bucket                         Cloud Storage
        pci-audit                    pci10-admin-activity
             │                                   │
             │                              Autoclass
             │                                   │
             ▼                                   ▼
       Logs Explorer                    Long-term storage
       / Analytics
```

Essa arquitetura cria uma separação clara entre **investigação operacional** e **retenção histórica**.

---

## Referência rápida

| Recurso    | Nome                           | Função                                           |
| ---------- | ------------------------------ | ------------------------------------------------ |
| Log Bucket | `_Required`                    | Armazenamento obrigatório do Cloud Logging       |
| Log Bucket | `_Default`                     | Armazenamento padrão                             |
| Log Bucket | `pci-audit`                    | Logs selecionados para auditoria                 |
| Sink       | `pci10-admin-activity-logging` | Envia Admin Activity para `pci-audit`            |
| Sink       | `pci10-admin-activity-gcs`     | Envia Admin Activity para GCS                    |
| GCS Bucket | `pci10-admin-activity`         | Armazenamento histórico                          |
| Autoclass  | no GCS                         | Otimização automática da classe de armazenamento |

## Referências oficiais

* [Cloud Logging: Log routing](https://docs.cloud.google.com/logging/docs/routing/overview?utm_source=chatgpt.com)
* [Cloud Logging: Store and route log entries](https://docs.cloud.google.com/logging/docs/store-log-entries?utm_source=chatgpt.com)
* [Cloud Logging: Export logs to Cloud Storage](https://docs.cloud.google.com/logging/docs/export/storage?utm_source=chatgpt.com)
* [Cloud Storage: Autoclass](https://docs.cloud.google.com/storage/docs/autoclass?utm_source=chatgpt.com)
* [Cloud Storage: Storage classes](https://docs.cloud.google.com/storage/docs/storage-classes?utm_source=chatgpt.com)

---
Source: #source/internet_resources
Project: #project/compliance 
Areas: #area/work
Subject: #subect/pci-dss 
Type: #type/idea
Learning priority: #priority/P2 
Status: #status/learned 
Related: [[PCI-DSS-GCP-GKE]]
