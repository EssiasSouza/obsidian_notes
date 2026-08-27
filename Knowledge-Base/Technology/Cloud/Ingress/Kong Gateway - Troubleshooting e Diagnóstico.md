# Kong Gateway - Troubleshooting e Diagnóstico
Guia rápido para diagnóstico de **processos, configuração, saúde, recursos, logs, Admin API, conectividade e backend** do Kong Gateway.

---

## 1. Processo e Status

### Verificar o processo do Kong

```bash
ps -ef | grep kong | grep -v grep
```

### Verificar a versão do Kong

```bash
kong version
```

### Verificar o status via systemd

> Aplicável quando o Kong é gerenciado pelo `systemd`.

```bash
systemctl status kong --no-pager
```

### Verificar a saúde do Kong

```bash
kong health
```

---

## 2. Configuração

### Validar a configuração do Kong

```bash
kong check /etc/kong/kong.conf
```

---

## 3. Portas e Conectividade Local

### Verificar portas abertas pelo Kong

```bash
ss -lntp | grep -E '8000|8001|8443|8444'
```

| Porta  | Função          |
| ------ | --------------- |
| `8000` | Proxy HTTP      |
| `8001` | Admin API HTTP  |
| `8443` | Proxy HTTPS     |
| `8444` | Admin API HTTPS |

---

## 4. Recursos do Sistema

### Verificar memória

```bash
free -h
```

### Verificar utilização do disco

```bash
df -h
```

### Verificar consumo de CPU e memória dos processos Nginx

```bash
ps -o pid,%cpu,%mem,cmd -C nginx
```

### Verificar utilização por thread

```bash
top -Hp $(pidof nginx)
```

---

# 5. Logs

Diretório padrão considerado neste documento:

```text
/usr/local/kong/logs/
```

## Error Log

### Visualizar os últimos erros

```bash
tail -100 /usr/local/kong/logs/error.log
```

### Acompanhar erros em tempo real

```bash
tail -f /usr/local/kong/logs/error.log
```

### Procurar erros críticos

```bash
grep -Ei "error|crit|alert|panic|fail|timeout" /usr/local/kong/logs/error.log | tail -100
```

### Procurar erros de um horário específico

Exemplo:

```bash
grep "15:41" /usr/local/kong/logs/error.log
```

### Procurar um Request ID específico

Substitua `<KONG_REQUEST_ID>` pelo ID da requisição:

```bash
grep "<KONG_REQUEST_ID>" /usr/local/kong/logs/error.log
```

Exemplos:

```bash
grep "7056dd7d7b02d212bd2bef4d5e86426c" /usr/local/kong/logs/error.log
```

```bash
grep "38fe910f11759a18182d606d12d3f559" /usr/local/kong/logs/error.log
```

```bash
grep "9208cf32730eaa5bcb324878f401da4c" /usr/local/kong/logs/error.log
```

---

## Access Log

### Visualizar as últimas requisições

```bash
tail -100 /usr/local/kong/logs/access.log
```

### Acompanhar requisições em tempo real

```bash
tail -f /usr/local/kong/logs/access.log
```

### Procurar requisições HTTP 502

```bash
grep ' 502 ' /usr/local/kong/logs/access.log | tail -20
```

---

# 6. Kong Admin API

> Os comandos abaixo consideram a Admin API disponível em `localhost:8001`.

### Verificar conectividade com a Admin API

```bash
curl -s http://localhost:8001/
```

### Listar Services

```bash
curl -s http://localhost:8001/services
```

### Listar Routes

```bash
curl -s http://localhost:8001/routes
```

### Listar Upstreams

```bash
curl -s http://localhost:8001/upstreams
```

### Verificar a saúde de um Upstream

Substitua `<UPSTREAM>` pelo nome ou ID do Upstream:

```bash
curl -s http://localhost:8001/upstreams/<UPSTREAM>/health
```

### Listar plugins habilitados

```bash
curl -s http://localhost:8001/plugins/enabled
```

---

# 7. Diagnóstico de Backend

## Testar conectividade HTTP

```bash
curl -vk http://<HOST>:<PORT>
```

## Testar conectividade HTTPS

```bash
curl -vk https://<HOST>:<PORT>
```

## Testar um endpoint específico

```bash
curl -vk http://<HOST>:<PORT>/<PATH>
```

Exemplo:

```bash
curl -vk https://backend.example.com:443/api/health
```

---

# 8. Diagnóstico de DNS

### Resolver DNS com nslookup

```bash
nslookup <HOSTNAME>
```

### Resolver DNS com dig

```bash
dig <HOSTNAME>
```

---

# 9. Diagnóstico de Conectividade TCP

### Testar uma porta TCP com netcat

```bash
nc -zv <HOST> <PORT>
```

### Alternativa usando telnet

```bash
telnet <HOST> <PORT>
```

---

# 10. Fluxo Rápido de Troubleshooting

Em caso de problema no Kong, seguir preferencialmente esta sequência:

### 1. Processo

```bash
ps -ef | grep kong | grep -v grep
```

### 2. Status

```bash
systemctl status kong --no-pager
```

### 3. Saúde

```bash
kong health
```

### 4. Configuração

```bash
kong check /etc/kong/kong.conf
```

### 5. Portas

```bash
ss -lntp | grep -E '8000|8001|8443|8444'
```

### 6. Recursos

```bash
free -h
```

```bash
df -h
```

### 7. Error Log

```bash
tail -100 /usr/local/kong/logs/error.log
```

### 8. Access Log

```bash
tail -100 /usr/local/kong/logs/access.log
```

### 9. Identificar HTTP 5xx

```bash
grep -E ' 5[0-9]{2} ' /usr/local/kong/logs/access.log | tail -50
```

### 10. Validar o backend

```bash
curl -vk https://<HOST>:<PORT>/<PATH>
```

### 11. Validar DNS

```bash
nslookup <HOSTNAME>
```

### 12. Validar conectividade TCP

```bash
nc -zv <HOST> <PORT>
```

---

# 11. Comandos Mais Utilizados

| Objetivo     | Comando                                         |
| ------------ | ----------------------------------------------- |
| Processo     | `ps -ef \| grep kong \| grep -v grep`           |
| Versão       | `kong version`                                  |
| Saúde        | `kong health`                                   |
| Configuração | `kong check /etc/kong/kong.conf`                |
| Status       | `systemctl status kong --no-pager`              |
| Portas       | `ss -lntp \| grep -E '8000\|8001\|8443\|8444'`  |
| Memória      | `free -h`                                       |
| Disco        | `df -h`                                         |
| Error Log    | `tail -100 /usr/local/kong/logs/error.log`      |
| Access Log   | `tail -100 /usr/local/kong/logs/access.log`     |
| Services     | `curl -s http://localhost:8001/services`        |
| Routes       | `curl -s http://localhost:8001/routes`          |
| Upstreams    | `curl -s http://localhost:8001/upstreams`       |
| Plugins      | `curl -s http://localhost:8001/plugins/enabled` |
| DNS          | `nslookup <HOSTNAME>`                           |
| TCP          | `nc -zv <HOST> <PORT>`                          |
