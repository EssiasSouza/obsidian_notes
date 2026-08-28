# Testar autenticação OAuth 2.0 com certificado dentro de um Pod Kubernetes

## Objetivo

Este procedimento mostra como acessar o container de uma aplicação executando em Kubernetes e realizar um teste manual de autenticação OAuth 2.0 utilizando:

* Certificado cliente (`.crt`)
* Chave privada (`.key`)
* HTTP Basic Authentication
* `grant_type=client_credentials`
* `curl`

O teste é útil para troubleshooting de integrações que utilizam **mTLS (Mutual TLS)**.

---

## 1. Identificar o Pod

Primeiro, liste os Pods do namespace:

```bash
kubectl get pods -n integration
```

Exemplo:

```text
NAME                                           READY   STATUS    RESTARTS   AGE
api-integration-service-7f8d9c6b5d-x7k2p       1/1     Running   0          2h
```

Neste exemplo, o Pod que será utilizado é:

```text
api-integration-service-7f8d9c6b5d-x7k2p
```

E o namespace:

```text
integration
```

---

## 2. Acessar o container

Se estiver utilizando Git Bash no Windows, utilize `//bin/bash` para evitar que o Git Bash converta o caminho Linux.

```bash
kubectl exec -it api-integration-service-7f8d9c6b5d-x7k2p -n integration -- //bin/bash
```

Se o container não possuir Bash, utilize:

```bash
kubectl exec -it api-integration-service-7f8d9c6b5d-x7k2p -n integration -- //bin/sh
```

Após entrar no container, o prompt deverá ser semelhante a:

```text
root@api-integration-service-7f8d9c6b5d-x7k2p:/app#
```

---

## 3. Verificar os certificados

Neste exemplo, os certificados estão montados no container através de Kubernetes Secrets.

Certificado:

```text
/var/secrets/ais_cert/gringo.crt
```

Chave privada:

```text
/var/secrets/ais_key/gringo.key
```

Verifique se os arquivos existem:

```bash
ls -l /var/secrets/ais_cert/ /var/secrets/ais_key/
```

Exemplo:

```text
/var/secrets/ais_cert/:
total 0
lrwxrwxrwx 1 root root 18 Aug 28 20:13 gringo.crt -> ..data/gringo.crt

/var/secrets/ais_key/:
total 0
lrwxrwxrwx 1 root root 17 Aug 28 20:13 gringo.key -> ..data/gringo.key
```

Esses links simbólicos são normais quando o Secret é montado como volume no Kubernetes.

---

## 4. Validar o certificado

Para verificar se o certificado pode ser lido:

```bash
openssl x509 -in /var/secrets/ais_cert/gringo.crt -text -noout
```

Também é possível verificar a validade:

```bash
openssl x509 -in /var/secrets/ais_cert/gringo.crt -noout -dates
```

Exemplo:

```text
notBefore=Aug 28 00:00:00 2026 GMT
notAfter=Aug 28 23:59:59 2027 GMT
```

---

## 5. Verificar a chave privada

Verifique se a chave pode ser lida:

```bash
openssl pkey -in /var/secrets/ais_key/gringo.key -check -noout
```

Se estiver correta, deverá aparecer algo semelhante a:

```text
Key is valid
```

---

## 6. Executar o teste OAuth

Agora execute o `curl` utilizando o certificado e a chave.

Exemplo fictício:

```bash
curl -sS --cert /var/secrets/ais_cert/gringo.crt --key /var/secrets/ais_key/gringo.key --header "Authorization: Basic <BASE64_CLIENT_ID_CLIENT_SECRET>" --header "Content-Type: application/x-www-form-urlencoded" --data "grant_type=client_credentials&scope=api://00000000-1111-2222-3333-444444444444/.default" "https://sandbox.example.com.br/v1/oauth2/token"
```

### Parâmetros utilizados

| Parâmetro       | Função                                                         |
| --------------- | -------------------------------------------------------------- |
| `--cert`        | Informa o certificado cliente                                  |
| `--key`         | Informa a chave privada correspondente                         |
| `Authorization` | Envia as credenciais do cliente em Basic Authentication        |
| `Content-Type`  | Informa que o body utiliza `application/x-www-form-urlencoded` |
| `grant_type`    | Define o fluxo OAuth 2.0 como Client Credentials               |
| `scope`         | Define o escopo solicitado                                     |
| URL             | Endpoint responsável por emitir o token                        |

---

## 7. Exemplo completo

Considerando:

```text
Pod: api-integration-service-7f8d9c6b5d-x7k2p
Namespace: integration
Certificado: /var/secrets/ais_cert/client.crt
Chave: /var/secrets/ais_key/client.key
Endpoint: https://sandbox.example.com.br/v1/oauth2/token
```

O procedimento completo seria:

```bash
kubectl exec -it api-integration-service-7f8d9c6b5d-x7k2p -n integration -- //bin/bash
```

Depois, dentro do container:

```bash
ls -l /var/secrets/ais_cert/ /var/secrets/ais_key/
```

E então:

```bash
curl -sS --cert /var/secrets/ais_cert/client.crt --key /var/secrets/ais_key/client.key --header "Authorization: Basic <BASE64_CLIENT_ID_CLIENT_SECRET>" --header "Content-Type: application/x-www-form-urlencoded" --data "grant_type=client_credentials&scope=api://00000000-1111-2222-3333-444444444444/.default" "https://sandbox.example.com.br/v1/oauth2/token"
```

---

## 8. Resultado esperado

Quando a autenticação for realizada com sucesso, o endpoint deverá retornar uma resposta JSON semelhante a:

```json
{
  "access_token": "eyJhbGciOiJSUzI1NiIs...",
  "token_type": "Bearer",
  "expires_in": 3600
}
```

Isso indica que:

1. O container possui acesso à rede.
2. O certificado cliente foi carregado.
3. A chave privada foi carregada.
4. O certificado e a chave são aceitos pelo endpoint.
5. As credenciais OAuth foram aceitas.
6. O endpoint conseguiu emitir um access token.

---

## 9. Troubleshooting

### Erro: arquivo não encontrado

Exemplo:

```text
curl: (58) could not load PEM client certificate
No such file or directory
```

Verifique os caminhos:

```bash
ls -l /var/secrets/ais_cert/
ls -l /var/secrets/ais_key/
```

Utilize caminhos absolutos no `curl`:

```text
/var/secrets/ais_cert/client.crt
/var/secrets/ais_key/client.key
```

Evite:

```text
./var/secrets/ais_cert/client.crt
```

pois, se o diretório atual for `/app`, esse caminho será interpretado como:

```text
/app/var/secrets/ais_cert/client.crt
```

---

### Erro relacionado ao certificado

Verifique o certificado:

```bash
openssl x509 -in /var/secrets/ais_cert/client.crt -text -noout
```

---

### Erro relacionado à chave privada

Verifique a chave:

```bash
openssl pkey -in /var/secrets/ais_key/client.key -check -noout
```

---

### Erro HTTP 401 ou 403

Nesse caso, o `curl` conseguiu chegar ao endpoint, mas a autenticação/autorização foi rejeitada.

Verifique:

* Client ID
* Client Secret
* Header `Authorization`
* Scope
* Validade do certificado
* Certificado utilizado no ambiente correto
* Permissões do client
* Endpoint utilizado

---

### Erro HTTP 400

Verifique principalmente o body enviado:

```text
grant_type=client_credentials
scope=api://<APPLICATION_ID>/.default
```

Também confirme se o endpoint espera exatamente esses parâmetros.

---

## 10. Importante sobre segurança

Não compartilhe em evidências, tickets ou documentação:

* Client Secret
* Chave privada `.key`
* Access Token
* Authorization Header contendo credenciais reais
* Conteúdo completo de Secrets

Para documentação, substitua os valores reais por placeholders:

```text
<CLIENT_ID>
<CLIENT_SECRET>
<BASE64_CLIENT_ID_CLIENT_SECRET>
<APPLICATION_ID>
```

O certificado público `.crt` geralmente não possui o mesmo nível de sensibilidade da chave privada, mas ainda assim deve ser tratado de acordo com as políticas de segurança da organização.

## Resumo

O fluxo utilizado neste troubleshooting é:

```text
kubectl
   |
   v
Pod Kubernetes
   |
   v
Container
   |
   +--> certificado .crt
   |
   +--> chave privada .key
   |
   v
curl
   |
   | mTLS + Basic Auth
   v
OAuth Token Endpoint
   |
   v
Access Token
```

---
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
