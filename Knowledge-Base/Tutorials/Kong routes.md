# Tutorial: Alterando uma Route do Kong pela Admin API

# Alterando um endpoint no Kong com segurança

## Objetivo

Este tutorial mostra como localizar uma Route no Kong a partir de um endpoint conhecido e alterar um dos paths dessa Route sem remover acidentalmente os demais.

Para os exemplos, vamos imaginar que uma aplicação chamada `travel-support-service` alterou seu endpoint de:

```text
/travel-assistance/v1/eligibility
```

para:

```text
/travel/v1/eligibility
```

Nossa tarefa será localizar a Route correspondente e atualizar o Kong.

---

## 1. Entendendo a estrutura

Antes de alterar qualquer coisa, é importante entender que no Kong temos uma diferença entre **Route** e **Path**.

Uma Route possui um ID próprio e pode ter um ou vários paths.

Por exemplo:

```json
{
  "id": "8f6a2d11-5c41-4f5a-91e7-72kas97ra40129",
  "name": "travel-support",
  "paths": [
    "~/travel-support/v1/payment",
    "~/travel-assistance/v1/eligibility"
  ]
}
```

Neste exemplo existe:

```text
1 Route
├── /travel-support/v1/payment
└── /travel-assistance/v1/eligibility
```

Portanto, quando precisamos alterar um endpoint, não devemos assumir que cada path representa uma Route diferente.

---

# 2. Procurando a Route pelo endpoint

Imagine que recebemos a informação:

```text
O endpoint /travel-assistance/v1/eligibility foi alterado.
```

Primeiro vamos procurar esse termo nas Routes existentes.

Podemos consultar as Routes do Kong e utilizar o `jq` para encontrar aquelas que possuem o path desejado:

`curl -s 'http://127.0.0.1:8001/routes?size=1000' | jq --arg TARGET '/travel-assistance/v1/eligibility' '.data[] | select(.paths[]? | contains($TARGET))'`

O resultado poderá ser parecido com:

```json
{
  "id": "8f6a2d11-5c41-4f5a-91e7-72kas97ra40129",
  "name": "travel-support",
  "paths": [
    "~/travel-support/v1/payment",
    "~/travel-assistance/v1/eligibility"
  ],
  "methods": [
    "GET"
  ]
}
```

Agora sabemos que o endpoint pertence à Route:

```text
8f6a2d11-5c41-4f5a-91e7-72kas97ra40129
```

---

# 3. Antes de alterar, consulte a Route diretamente

Nunca é uma boa ideia alterar uma Route apenas com base no resultado de uma busca.

Depois de encontrar o ID, consulte a Route diretamente:

`curl -s 'http://127.0.0.1:8001/routes/8f6a2d11-5c41-4f5a-91e7-72c3d8a40129' | jq`

Vamos imaginar que encontramos:

```json
{
  "id": "8f6a2d11-5c41-4f5a-91e7-72kas97ra40129",
  "name": "travel-support",
  "paths": [
    "~/travel-support/v1/payment",
    "~/travel-assistance/v1/eligibility"
  ]
}
```

Aqui temos uma informação muito importante.

A Route possui **dois paths**:

```text
~/travel-support/v1/payment
~/travel-assistance/v1/eligibility
```

Queremos alterar somente o segundo.

Portanto, não devemos enviar um PATCH contendo somente o novo endpoint.

---

# 4. Alterando o endpoint

O novo endpoint será:

```text
/travel/v1/eligibility
```

Como a propriedade `paths` representa o conjunto de paths da Route, devemos enviar novamente os paths que queremos manter.

Neste exemplo:

```json
{
  "paths": [
    "~/travel-support/v1/payment",
    "~/travel/v1/eligibility"
  ]
}
```

O comando será:

`curl -s -X PATCH 'http://127.0.0.1:8001/routes/8f6a2d11-5c41-4f5a-91e7-72c3d8a40129' -H 'Content-Type: application/json' --data '{"paths":["~/travel-support/v1/payment","~/travel/v1/eligibility"]}' | jq`

---

# 5. Validando a alteração

Depois do PATCH, consulte novamente a Route:

`curl -s 'http://127.0.0.1:8001/routes/8f6a2d11-5c41-4f5a-91e7-72c3d8a40129' | jq '.paths'`

O resultado esperado será:

```json
[
  "~/travel-support/v1/payment",
  "~/travel/v1/eligibility"
]
```

O path antigo:

```text
~/travel-assistance/v1/eligibility
```

não deve mais aparecer.

O novo path:

```text
~/travel/v1/eligibility
```

deve estar presente.

---

# 6. Uma maneira ainda melhor de investigar

Durante troubleshooting, muitas vezes queremos visualizar somente informações relevantes das Routes.

Podemos retornar o ID, o nome e os paths:

`curl -s 'http://127.0.0.1:8001/routes?size=1000' | jq -r '.data[] | {id, name, paths}'`

Isso facilita bastante a análise quando existem muitas Routes.

Também podemos procurar diretamente pelo termo e mostrar somente o ID e os paths:

`curl -s 'http://127.0.0.1:8001/routes?size=1000' | jq --arg TARGET '/travel-assistance/v1/eligibility' '.data[] | select(.paths[]? | contains($TARGET)) | {id, name, paths}'`

---

# 7. O erro que devemos evitar

Um erro comum seria encontrar uma Route com dois paths:

```text
~/travel-support/v1/payment
~/travel-assistance/v1/eligibility
```

e executar:

`curl -s -X PATCH 'http://127.0.0.1:8001/routes/8f6a2d11-5c41-4f5a-91e7-72c3d8a40129' -H 'Content-Type: application/json' --data '{"paths":["~/travel/v1/eligibility"]}' | jq`

Isso pode deixar a Route somente com:

```text
~/travel/v1/eligibility
```

O outro path:

```text
~/travel-support/v1/payment
```

também seria removido da configuração da Route.

Por isso, antes de executar um PATCH em `paths`, precisamos saber **quais paths já existem naquela Route**.

---

# 8. Fluxo recomendado

Para esse tipo de alteração, o fluxo pode ser resumido em cinco etapas:

```text
Endpoint conhecido
       │
       ▼
Pesquisar nas Routes
       │
       ▼
Encontrar o Route ID
       │
       ▼
Consultar a Route
       │
       ▼
Identificar todos os paths
       │
       ▼
Alterar somente o path necessário
       │
       ▼
Validar a configuração
```

A ideia principal é simples:

> **Não altere uma Route antes de saber o que existe dentro dela.**

O endpoint que recebemos como informação de mudança é o ponto de partida para encontrar a Route. O `Route ID` é então utilizado para consultar e modificar a configuração correta.

---

# 9. Checklist

Antes do PATCH:

* [ ] Tenho o endpoint antigo que preciso localizar.
* [ ] Pesquisei esse endpoint nas Routes.
* [ ] Identifiquei o `Route ID`.
* [ ] Consultei a Route diretamente pelo ID.
* [ ] Sei quais paths pertencem à Route.
* [ ] Identifiquei exatamente qual path será alterado.
* [ ] Mantive os demais paths no payload do PATCH.

Depois do PATCH:

* [ ] Consultei novamente a Route.
* [ ] O path antigo não está mais presente.
* [ ] O novo path está presente.
* [ ] Os demais paths continuam presentes.
* [ ] A configuração está de acordo com a mudança solicitada.

## Conclusão

Alterar um endpoint no Kong não significa simplesmente criar uma nova Route.

O primeiro passo é descobrir **qual Route atende aquele endpoint**. Depois, devemos verificar a configuração existente e alterar somente o que realmente mudou.

Essa abordagem reduz bastante o risco de modificar ou remover acidentalmente outros endpoints que compartilham a mesma Route.

---
Source: #source/internet_resources 
Project: #project/infrastructure
Areas: #area/work
Subject: #subect/api_gateway
Type: #type/learning 
Learning priority: #priority/P2 
Status: #status/to_learning
Related: [[Kong]]
