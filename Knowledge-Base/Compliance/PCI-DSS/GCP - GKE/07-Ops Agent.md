Source: #source/internet_resources
Project: #project/compliance
Areas: #area/work
Subject: #subject/pci-dss
Type: #type/learning
Learning priority: #priority/P2
Status: #status/learning
Related: [[PCI-DSS-GCP-GKE]]

---

# Ops Agent

## Objetivo

O **Google Cloud Ops Agent** é o agente oficial da Google para coleta de **logs** e **métricas** em instâncias do Compute Engine.

Para ambientes sujeitos ao PCI-DSS, ele permite coletar eventos que acontecem **dentro do sistema operacional**, como:

- Syslog (Linux)
- Windows Event Log
- Logs de aplicações
- Métricas do sistema operacional

Esses dados são enviados para:

- Cloud Logging
- Cloud Monitoring

O Ops Agent substitui os antigos **Logging Agent** e **Monitoring Agent**, unificando a coleta em um único agente. :contentReference[oaicite:0]{index=0}

---

# Requisitos PCI-DSS relacionados

- PCI DSS 10.2.1

---

# Arquitetura

```text
Compute Engine VM
        │
        │
 Google Cloud Ops Agent
        │
 ├──────────────┐
 │              │
 ▼              ▼
Cloud Logging   Cloud Monitoring
```

---

# Pré-requisitos

Antes da instalação, verifique:

- A VM está no Compute Engine.
- O sistema operacional é suportado.
- As APIs abaixo estão habilitadas:
  - Cloud Logging API
  - Cloud Monitoring API
- A Service Account da VM possui, no mínimo, os papéis:
  - Logs Writer (`roles/logging.logWriter`)
  - Monitoring Metric Writer (`roles/monitoring.metricWriter`)
- Os antigos **Logging Agent** e **Monitoring Agent** não estão instalados, evitando duplicidade de logs e métricas. :contentReference[oaicite:1]{index=1}

---

# Instalação no Linux

Acesse a VM utilizando SSH.

Baixe o script oficial:

```bash
curl -sSO https://dl.google.com/cloudagents/add-google-cloud-ops-agent-repo.sh
```

Instale o agente:

```bash
sudo bash add-google-cloud-ops-agent-repo.sh --also-install
```

Após a instalação, o serviço é iniciado automaticamente. :contentReference[oaicite:2]{index=2}

---

# Instalação no Windows

Abra um PowerShell como Administrador.

Execute:

```powershell
(New-Object Net.WebClient).DownloadFile(
"https://dl.google.com/cloudagents/add-google-cloud-ops-agent-repo.ps1",
"${env:UserProfile}\add-google-cloud-ops-agent-repo.ps1"
)

Invoke-Expression "${env:UserProfile}\add-google-cloud-ops-agent-repo.ps1 -AlsoInstall"
```

Após a instalação, o serviço é iniciado automaticamente. :contentReference[oaicite:3]{index=3}

---

# Verificando a instalação

## Linux

Verifique o status do serviço.

```bash
sudo systemctl status google-cloud-ops-agent
```

O resultado esperado é que os componentes **Logging Agent** e **Metrics Agent** estejam em execução. :contentReference[oaicite:4]{index=4}

---

## Windows

```powershell
Get-Service google-cloud-ops-agent
```

O status esperado é:

```text
Running
```

:contentReference[oaicite:5]{index=5}

---

# Reiniciando o agente

## Linux

```bash
sudo systemctl restart google-cloud-ops-agent
```

---

## Windows

```powershell
Restart-Service google-cloud-ops-agent -Force
```

:contentReference[oaicite:6]{index=6}

---

# Atualizando o agente

Para atualizar para a versão mais recente:

```bash
sudo bash add-google-cloud-ops-agent-repo.sh --also-install
```

Também é possível instalar uma versão específica utilizando o parâmetro `--version`. :contentReference[oaicite:7]{index=7}

---

# O que o Ops Agent coleta por padrão

## Logs

No Linux:

- Syslog

No Windows:

- Windows Event Log

Além disso, o agente pode ser configurado para coletar logs de aplicações específicas utilizando um arquivo YAML de configuração. :contentReference[oaicite:8]{index=8}

---

## Métricas

São coletadas automaticamente métricas da VM, incluindo:

- CPU
- Memória
- Disco
- Rede

Essas métricas são enviadas ao Cloud Monitoring. :contentReference[oaicite:9]{index=9}

---

# Arquivo de configuração

O Ops Agent utiliza um único arquivo YAML para configuração.

Linux:

```text
/etc/google-cloud-ops-agent/config.yaml
```

Windows:

```text
C:\Program Files\Google\Cloud Operations\Ops Agent\config\config.yaml
```

Após qualquer alteração nesse arquivo, reinicie o serviço do agente. :contentReference[oaicite:10]{index=10}

---

# Evidências sugeridas

Para comprovação durante uma auditoria, recomenda-se registrar:

- Instância do Compute Engine com o Ops Agent instalado.
- Status do serviço em execução.
- Logs chegando ao Cloud Logging.
- Métricas disponíveis no Cloud Monitoring.
- Arquivo `config.yaml` configurado, quando houver customizações.

---

# Boas práticas

- Utilizar apenas o **Ops Agent**, evitando a coexistência com os agentes legados.
- Manter o agente atualizado.
- Validar periodicamente que o serviço permanece em execução.
- Após alterações no arquivo de configuração, reiniciar o agente para aplicar as mudanças. :contentReference[oaicite:11]{index=11}

---

# Referências

- :contentReference[oaicite:12]{index=12}
- :contentReference[oaicite:13]{index=13}
- :contentReference[oaicite:14]{index=14}