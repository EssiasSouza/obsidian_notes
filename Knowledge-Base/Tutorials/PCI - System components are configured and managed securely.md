Source: #source/internet_resources
Project: #project/devops 
Areas: #area/work
Subject: #subject/pci-dss
Type: #type/idea
Learning priority: #priority/P0 
Status: #status/learning 
Related: [[GKE - Google Kubernetes Engine]]
[[Cyber Security]]
[[Cloud Computing]]
[[GCP - Google Cloud Platform]]

---

# PCI - System components are configured and managed securely

O que a auditoria está pedindo é, na prática, evidências de que **cada workload possui uma função bem definida e que workloads com perfis de segurança diferentes estão isolados**.

No seu caso, como você utiliza **GKE Autopilot**, algumas responsabilidades são do Google e outras são suas. A auditoria normalmente quer avaliar as suas responsabilidades dentro do modelo de responsabilidade compartilhada.

---

# O que o auditor quer verificar

Ele quer responder perguntas como:

* Existem containers fazendo mais de uma função?
* Existe um Pod que roda aplicação + banco de dados?
* Existe um Pod rodando nginx + sshd + cron?
* Aplicações públicas estão separadas das internas?
* Existe isolamento entre ambientes?
* Existe isolamento entre aplicações PCI e não PCI?
* Existem Network Policies?
* Existem controles para impedir que um container afete outro?

Em Kubernetes isso é diferente de servidores tradicionais.

---

# O que verificar no GKE

## 1. Um Deployment possui apenas uma função

Liste todos os deployments.

```bash
kubectl get deploy -A
```

Depois veja cada um.

```bash
kubectl describe deploy <deployment> -n <namespace>
```

ou

```bash
kubectl get deploy <deployment> \
-n <namespace> \
-o yaml
```

Verifique:

* Quantos containers existem

Por exemplo

```yaml
containers:

- name: api

- name: cloud-sql-proxy
```

Isso normalmente é aceitável.

Já algo assim

```yaml
containers:

- nginx

- mysql

- redis

- ssh
```

provavelmente seria questionado.

---

## 2. Containers possuem apenas um processo principal

Entre em um Pod.

```bash
kubectl exec -it POD -n namespace -- sh
```

Depois

```bash
ps aux
```

ou

```bash
ps -ef
```

Idealmente verá algo como

```
PID COMMAND

1 java

32 sh
```

e não

```
nginx

sshd

cron

mysqld

apache
```

---

## 3. Existem bancos rodando dentro do cluster?

Liste os Pods.

```bash
kubectl get pods -A
```

Procure por

```
mysql

postgres

redis

mongodb

oracle

mssql
```

Se existirem, veja se realmente deveriam existir.

Em GKE normalmente bancos ficam em

* Cloud SQL
* AlloyDB
* Bigtable

e não dentro do cluster.

---

# 4. Separação por Namespace

Liste

```bash
kubectl get ns
```

Idealmente algo parecido com

```
prod

dev

monitoring

logging

ingress

pci

external-secrets
```

Isso mostra separação lógica.

---

# 5. Aplicações PCI ficam isoladas

Se apenas algumas aplicações processam cartão, elas deveriam estar em namespace separado.

Por exemplo

```
pci-payments

pci-api

pci-auth
```

e não misturadas com

```
marketing

backoffice

cms
```

---

# 6. Network Policies

Muito importante para PCI.

Veja

```bash
kubectl get networkpolicy -A
```

ou

```bash
kubectl get netpol -A
```

Se aparecer

```
No resources found
```

significa que qualquer Pod pode conversar com qualquer outro.

Isso normalmente gera observação em PCI.

---

# 7. Ingress públicos x internos

Veja

```bash
kubectl get ingress -A
```

ou

```bash
kubectl get svc -A
```

Procure

```
LoadBalancer
```

Veja quais possuem IP público.

```
kubectl get svc -A -o wide
```

Você deve conseguir explicar:

* quais são públicos
* quais são internos

---

# 8. Contas de serviço

Cada aplicação deve usar sua própria identidade.

```bash
kubectl get sa -A
```

Depois

```bash
kubectl get deploy -A \
-o jsonpath='{range .items[*]}{.metadata.namespace}{" "}{.metadata.name}{" "}{.spec.template.spec.serviceAccountName}{"\n"}{end}'
```

Evite tudo usando

```
default
```

---

# 9. Permissões

Veja RBAC.

```bash
kubectl get role -A

kubectl get rolebinding -A

kubectl get clusterrole

kubectl get clusterrolebinding
```

---

# 10. Security Context

Verifique

```bash
kubectl get pod POD \
-o yaml
```

Procure

```yaml
securityContext:
```

Idealmente

```yaml
runAsNonRoot: true

allowPrivilegeEscalation: false

readOnlyRootFilesystem: true

seccompProfile:
  type: RuntimeDefault
```

---

# 11. Containers privilegiados

Muito importante.

Liste todos

```bash
kubectl get pods -A -o yaml | grep privileged
```

Idealmente

```
false
```

---

# 12. Host Network

Veja

```bash
kubectl get pods -A -o yaml | grep hostNetwork
```

Idealmente

```
false
```

---

# 13. Host PID

Veja

```bash
kubectl get pods -A -o yaml | grep hostPID
```

Idealmente

```
false
```

---

# 14. HostPath

Muito importante.

```bash
kubectl get pods -A -o yaml | grep hostPath
```

Idealmente nenhum.

---

# 15. DaemonSets

Liste

```bash
kubectl get daemonset -A
```

Normalmente você verá

* CNI
* métricas
* observabilidade

Não deveria haver aplicações de negócio rodando como DaemonSet sem justificativa.

---

# Evidências para entregar ao auditor

Uma boa resposta para esse requisito geralmente inclui:

| Evidência                   | Como obter                 |
| --------------------------- | -------------------------- |
| Namespaces separados        | `kubectl get ns`           |
| Deployments por aplicação   | `kubectl get deploy -A`    |
| Network Policies            | `kubectl get netpol -A`    |
| Service Accounts dedicadas  | `kubectl get sa -A`        |
| Ingress públicos e internos | `kubectl get ingress -A`   |
| Security Context dos Pods   | `kubectl get pod -o yaml`  |
| Containers privilegiados    | Busca por `privileged`     |
| Ausência de `hostPath`      | Busca por `hostPath`       |
| Ausência de `hostNetwork`   | Busca por `hostNetwork`    |
| DaemonSets instalados       | `kubectl get daemonset -A` |

## Particularidades do GKE Autopilot

O **GKE Autopilot** já aplica diversos controles que fortalecem a conformidade com esse requisito do PCI DSS. Por padrão, ele restringe o uso de containers privilegiados, `hostNetwork`, `hostPID`, `hostIPC` e muitos tipos de volumes `hostPath`, além de aplicar políticas de admissão baseadas nos padrões do Kubernetes. Isso reduz significativamente o risco de uma aplicação comprometer outras funções no cluster.

Ainda assim, continuam sendo responsabilidade da sua equipe aspectos como:

* Definir uma arquitetura com separação lógica por namespaces.
* Implementar e manter `NetworkPolicy` para controlar a comunicação entre workloads.
* Utilizar contas de serviço (Kubernetes e IAM) com privilégio mínimo.
* Garantir que aplicações PCI e não PCI estejam segregadas quando exigido pelo escopo da auditoria.
* Configurar corretamente Ingress, Services e regras de exposição à Internet.
* Validar que cada workload desempenha apenas sua função principal e não executa serviços desnecessários.

Pela sua descrição anterior do ambiente, com **GKE Autopilot**, **External Secrets Operator**, **Secret Manager** e bancos de dados gerenciados fora do cluster, você provavelmente já atende boa parte desse requisito. O ponto que costuma gerar mais observações em auditorias de PCI em ambientes Kubernetes é a **microsegmentação entre aplicações**, normalmente implementada por meio de `NetworkPolicy`, além da comprovação de que workloads do escopo PCI estão devidamente isolados dos demais.
