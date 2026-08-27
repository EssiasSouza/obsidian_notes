Source: #source/internet_resources 
Project: #project/devops
Areas: #area/work
Subject: #subject/kubernetes
Type: #type/tutorial
Status: #status/learning 
Related: [[Cloud Computing]]
[[Kubernetes Roadmap]]
[[External Secret Operator]]
[[GKE - Google Kubernetes Engine]]

---
# Tutorial: Corrigindo o warning de `version` no provider do Terraform

## Objetivo

Corrigir o warning:

```text
Warning: Version constraints inside provider configuration blocks are deprecated
```

Esse warning acontece quando a versão do provider é definida diretamente dentro do bloco `provider`.

---

## 1. Identifique a configuração atual

Procure no arquivo `provider.tf` por algo semelhante a:

```hcl
provider "google" {
  project = var.environment == "prd" ? var.project["prd"] : (var.environment == "staging" ? var.project["staging"] : (var.environment == "dr" ? var.project["dr"] : var.project["staging"]))
  region  = var.region
  zone    = "southamerica-east1-b"
  version = "~> 6.0"
}
```

O problema está nesta linha:

```hcl
version = "~> 6.0"
```

A restrição de versão não deve ficar dentro do bloco `provider`.

---

## 2. Remova a versão do `provider`

O bloco deve ficar assim:

```hcl
provider "google" {
  project = var.environment == "prd" ? var.project["prd"] : (var.environment == "staging" ? var.project["staging"] : (var.environment == "dr" ? var.project["dr"] : var.project["staging"]))
  region  = var.region
  zone    = "southamerica-east1-b"
}
```

---

## 3. Crie ou altere o `versions.tf`

É recomendado centralizar as versões dos providers em um arquivo como `versions.tf`.

Adicione:

```hcl
terraform {
  required_providers {
    google = {
      source  = "hashicorp/google"
      version = "~> 6.0"
    }
  }
}
```

A versão continua sendo `~> 6.0`, mas agora está no local correto.

---

## 4. Inicialize novamente o Terraform

Após realizar a alteração, execute:

```bash
terraform init
```

Se necessário, para atualizar as dependências:

```bash
terraform init -upgrade
```

**Atenção:** `-upgrade` pode atualizar providers dentro das constraints permitidas. Portanto, em ambientes produtivos, avalie o impacto antes de utilizá-lo.

---

## 5. Valide a configuração

Execute:

```bash
terraform validate
```

E, opcionalmente:

```bash
terraform plan
```

O warning relacionado à versão dentro do bloco `provider` não deverá mais aparecer.

---

## Resultado esperado

### Antes

```hcl
provider "google" {
  project = var.project
  region  = var.region
  zone    = "southamerica-east1-b"
  version = "~> 6.0"
}
```

### Depois

**`provider.tf`**

```hcl
provider "google" {
  project = var.project
  region  = var.region
  zone    = "southamerica-east1-b"
}
```

**`versions.tf`**

```hcl
terraform {
  required_providers {
    google = {
      source  = "hashicorp/google"
      version = "~> 6.0"
    }
  }
}
```

### Resumo

|Configuração|Local correto|
|---|---|
|`project`|`provider`|
|`region`|`provider`|
|`zone`|`provider`|
|`version`|`terraform.required_providers`|
|`source`|`terraform.required_providers`|

**Regra para lembrar:** o bloco `provider` configura **como o provider funciona**; o bloco `required_providers` define **qual provider e qual versão serão utilizados**.