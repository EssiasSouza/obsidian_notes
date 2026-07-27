
|Característica|Logging Bucket|Cloud Storage (GCS)|
|:--|:--|:--|
|**Melhor para**|Análise em tempo real, depuração e alertas.|Conformidade e auditoria de longo prazo.|
|**Busca/Pesquisa**|Logs Explorer e Log Analytics (SQL) integrados.|Difícil (precisa baixar os arquivos ou importar para o BigQuery).|
|**Custo de Ingestão**|Alto (US$ 0,50/GiB).|Baixo/Zero.|
|**Custo de Retenção**|Moderado (US$ 0,01/GiB).|Muito baixo (nas classes Coldline/Archive).|

### Estratégia Recomendada

A melhor abordagem depende da necessidade de consulta:

- **Opção A (Foco em Economia):** Se você raramente consulta os logs de _Data Access_ e só os mantém por compliance, envie-os **apenas para o Cloud Storage**. Se precisar investigar algo, você pode importar o arquivo de volta ou usar o BigQuery para ler o bucket.
- **Opção B (Equilíbrio):** Se você precisa investigar acessos recentes (dos últimos 30 dias), mantenha a retenção de 30 dias no Logging e crie um Sink para o GCS.
    - _Dica:_ Use filtros de exclusão no Logging para manter apenas os logs de _Data Access_ de serviços críticos no Logging Bucket, enviando o restante (o "grosso" dos dados) apenas para o GCS.

### Como funciona a "movimentação"

No Google Cloud, o roteamento é em tempo real. Para simular esse "mover após 30 dias", é necessário fazer o seguinte:

1. **No Logging:** Deixar os logs entrarem no bucket `_Default` (ou um personalizado) com retenção de 30 dias. Eles serão excluídos automaticamente após esse período.
2. **No Sink (Coletor):** Configurar o destino para o Cloud Storage **agora**. O log será gravado no GCS no mesmo instante em que é gerado. Ele ficará lá "esquecido" até que alguém precise dele no futuro, enquanto a cópia no Logging desaparece após 30 dias.