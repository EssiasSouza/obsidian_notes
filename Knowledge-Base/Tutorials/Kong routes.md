Source: #source/internet_resources 
Project: #project/infrastructure
Areas: #area/work
Subject: #subect/api_gateway
Type: #type/learning 
Learning priority: #priority/P2 
Status: #status/to_learning
Related: [[Kong]]

---
# Tutorial: Alterando uma Route do Kong pela Admin API

## 1. Cenário

Em determinado momento, uma aplicação pode alterar um endpoint e o Kong precisa ser atualizado para encaminhar as requisições para o novo path.

Exemplo fictício:

```text
ANTES:
/old-service/v1/eligibility

DEPOIS:
/new-service/v1/eligibility
```

Neste tutorial, vamos considerar:

```text
Ambiente: staging
Kong: rodando diretamente em uma VM
Kong version: 3.5.0
Banco: PostgreSQL externo
Admin API: porta 8001
```

A arquitetura será aproximadamente:

```text
Cliente
   |
   v
Kong Gateway
   |
   | Route
   v
Service
   |
   v
Aplicação
```

---

# 2. Descobrir como o Kong está rodando

Primeiro, acessar a VM onde o Kong está instalado.

Assumindo Linux:

```bash
sudo su
```

Verificar se o Kong está rodando diretamente no sistema:

```bash
ps -ef | grep [k]ong
```

Exemplo de resultado:

```text
root   1234   1  ... nginx: master process /usr/local/openresty/nginx/sbin/nginx -p /usr/local/kong -c nginx.conf
kong   1235   ... nginx: worker process
```

Se aparecer algo semelhante, o Kong está rodando diretamente na VM através do OpenResty/Nginx.

Também podemos verificar se existe Docker:

```bash
docker ps | grep -i kong
```

Se aparecer:

```text
Command 'docker' not found
```

isso indica que Docker não está instalado nessa VM. Isso não significa que o Kong não use containers em outro lugar, mas neste cenário específico ele está rodando diretamente na VM.

---

# 3. Verificar a versão do Kong

Consultar a versão:

```bash
/usr/local/bin/kong version
```

Exemplo:

```text
3.5.0
```

É importante saber a versão porque a estrutura e os recursos da Admin API podem variar entre versões.

---

# 4. Descobrir as portas utilizadas pelo Kong

Verificar as portas padrão do Kong:

```bash
ss -lntp | grep -E ':(8000|8001|8443|8444)\b'
```

Normalmente:

```text
8000  -> Proxy HTTP
8443  -> Proxy HTTPS
8001  -> Admin API HTTP
8444  -> Admin API HTTPS
```

Exemplo:

```text
0.0.0.0:8000
0.0.0.0:8443
0.0.0.0:8001
127.0.0.1:8444
```

Se a porta `8001` estiver disponível localmente, podemos consultar o Kong usando:

```text
http://127.0.0.1:8001
```

---

# 5. Confirmar que a Admin API está funcionando

Fazer uma requisição somente de leitura:

```bash
curl -s http://127.0.0.1:8001/
```

Se funcionar, o Kong retornará um JSON contendo informações como:

```json
{
  "version": "3.5.0",
  "hostname": "kong-staging",
  "tagline": "Welcome to Kong"
}
```

Isso confirma que a Admin API está respondendo.

---

# 6. Descobrir se existe banco PostgreSQL externo

A configuração do Kong pode ser consultada pela própria Admin API.

Exemplo:

```bash
curl -s http://127.0.0.1:8001/
```

No JSON retornado, procure por propriedades relacionadas a PostgreSQL, como:

```text
"database":"postgres"
"pg_host":"kong-db.staging.example.com"
"pg_database":"kong"
"pg_port":5432
"pg_user":"kong"
```

Se aparecer algo semelhante, significa que:

```text
Kong
  |
  v
PostgreSQL
  |
  +-- database: kong
```

Neste cenário, **não devemos editar as tabelas do PostgreSQL diretamente** para alterar Routes.

A preferência é utilizar a Admin API do Kong.

---

# 7. Procurar a Route pelo path

Quando não sabemos o nome exato do Service ou da Route, podemos consultar as Routes existentes.

Exemplo:

```bash
curl -s 'http://127.0.0.1:8001/routes?size=1000'
```

Como o retorno pode ser enorme, podemos procurar um trecho conhecido do endpoint:

```bash
curl -s 'http://127.0.0.1:8001/routes?size=1000' | grep -o '.\{0,200\}/old-service/v1/eligibility.\{0,500\}'
```

Exemplo de resultado:

```text
"paths":["~/old-service/contract/v1/payment/remember","~/old-service/v1/eligibility"],"created_at":...,"service":{"id":"abc123"},"id":"route-123"
```

Neste momento conseguimos identificar:

```text
Route ID:
route-123

Service ID:
abc123

Path antigo:
~/old-service/v1/eligibility
```

---

# 8. Consultar a Route completa antes de alterar

Nunca altere a configuração imediatamente após encontrar o ID.

Primeiro consulte a Route:

```bash
curl -s http://127.0.0.1:8001/routes/route-123
```

Exemplo:

```json
{
  "hosts": [
    "api-staging.example.com"
  ],
  "methods": [
    "GET"
  ],
  "paths": [
    "~/old-service/contract/v1/payment/remember",
    "~/old-service/v1/eligibility"
  ],
  "protocols": [
    "http",
    "https"
  ],
  "name": "old-service-contract",
  "service": {
    "id": "abc123"
  },
  "id": "route-123",
  "path_handling": "v1",
  "strip_path": false
}
```

Antes da alteração, observe principalmente:

```text
id
name
service.id
paths
methods
hosts
protocols
strip_path
path_handling
```

---

# 9. Cuidado quando uma Route possui várias paths

Esse é um ponto muito importante.

Uma única Route pode possuir vários paths.

Exemplo:

```json
"paths": [
  "~/old-service/contract/v1/payment/remember",
  "~/old-service/v1/eligibility"
]
```

Se precisamos alterar apenas:

```text
~/old-service/v1/eligibility
```

para:

```text
~/new-service/v1/eligibility
```

não devemos enviar apenas:

```json
"paths": [
  "~/new-service/v1/eligibility"
]
```

porque isso pode remover a outra configuração existente.

Devemos preservar as duas:

```json
"paths": [
  "~/old-service/contract/v1/payment/remember",
  "~/new-service/v1/eligibility"
]
```

---

# 10. Alterar a Route

Depois de confirmar todas as informações, podemos utilizar `PATCH`.

Exemplo fictício:

```bash
curl -s -X PATCH http://127.0.0.1:8001/routes/route-123 -H 'Content-Type: application/json' --data '{"paths":["~/old-service/contract/v1/payment/remember","~/new-service/v1/eligibility"]}'
```

Esse comando faz:

```text
PATCH
  |
  v
Route route-123
  |
  +-- mantém /old-service/contract/v1/payment/remember
  |
  +-- troca /old-service/v1/eligibility
      por /new-service/v1/eligibility
```

As demais propriedades da Route não são enviadas no `PATCH`, portanto permanecem como estavam.

---

# 11. Validar imediatamente após a alteração

Depois do `PATCH`, consulte novamente a Route:

```bash
curl -s http://127.0.0.1:8001/routes/route-123
```

Confirme que o retorno contém:

```json
"paths": [
  "~/old-service/contract/v1/payment/remember",
  "~/new-service/v1/eligibility"
]
```

E confirme que o path antigo desapareceu:

```text
~/old-service/v1/eligibility
```

---

# 12. Testar o endpoint pelo Kong

Depois de confirmar a configuração, teste o endpoint pelo endereço usado pelos consumidores.

Exemplo:

```text
https://api-staging.example.com/new-service/v1/eligibility
```

No teste, valide pelo menos:

```text
HTTP status
Resposta da aplicação
Tempo de resposta
Headers relevantes
Logs do Kong
```

Também é interessante verificar os logs do Kong:

```bash
tail -f /usr/local/kong/logs/access.log
```

E, caso exista algum problema:

```bash
tail -f /usr/local/kong/logs/error.log
```

---

# 13. Fluxo resumido para futuras alterações

Quando alguém pedir:

> "Altera o endpoint X para Y no Kong"

seguir esta sequência:

```text
1. Acessar a VM do Kong
       |
       v
2. Confirmar que o Kong está rodando
       |
       v
3. Confirmar a Admin API na porta 8001
       |
       v
4. Consultar as Routes
       |
       v
5. Encontrar o path atual
       |
       v
6. Obter o Route ID
       |
       v
7. Consultar a Route completa
       |
       v
8. Identificar todas as paths existentes
       |
       v
9. Fazer PATCH preservando as outras paths
       |
       v
10. Consultar a Route novamente
       |
       v
11. Testar o endpoint
       |
       v
12. Verificar logs
```

---

# 14. Checklist antes do PATCH

Antes de executar qualquer alteração, confirme:

```text
[ ] Estou na VM correta
[ ] Estou no ambiente correto
[ ] A Admin API é a do Kong esperado
[ ] Encontrei o Route ID correto
[ ] Confirmei o Service associado
[ ] Confirmei o path antigo
[ ] Sei qual será o novo path
[ ] Verifiquei se existem múltiplos paths na Route
[ ] Preservei os demais paths
[ ] Não estou alterando o banco PostgreSQL diretamente
```

---

# 15. Exemplo completo

Imagine que encontramos:

```text
Route ID:
12345678-abcd-1234-abcd-123456789abc

Service ID:
abcdef12-3456-7890-abcd-123456789abc
```

A Route atual:

```json
{
  "paths": [
    "~/customer-service/v1/payment",
    "~/customer-service/v1/eligibility"
  ],
  "methods": [
    "GET"
  ]
}
```

O desenvolvimento informou que o segundo endpoint mudou para:

```text
/customer-service/v2/eligibility
```

Então o PATCH deverá preservar o primeiro path:

```bash
curl -s -X PATCH http://127.0.0.1:8001/routes/12345678-abcd-1234-abcd-123456789abc -H 'Content-Type: application/json' --data '{"paths":["~/customer-service/v1/payment","~/customer-service/v2/eligibility"]}'
```

Depois:

```bash
curl -s http://127.0.0.1:8001/routes/12345678-abcd-1234-abcd-123456789abc
```

E verificar:

```text
/customer-service/v1/payment
/customer-service/v2/eligibility
```

---

# 16. Principal aprendizado

A parte mais importante desse procedimento é **não começar alterando o Kong**.

Primeiro:

```text
Descobrir
   ↓
Identificar
   ↓
Consultar
   ↓
Confirmar
   ↓
Alterar
   ↓
Validar
   ↓
Testar
```

No cenário que trabalhamos, descobrimos que:

```text
Kong
  ↓
OpenResty/Nginx
  ↓
Admin API :8001
  ↓
PostgreSQL externo
```

Por isso, a alteração correta foi feita pela **Kong Admin API**, e não diretamente no PostgreSQL ou nos arquivos do Nginx.
