# Verificar o processo do Kong
ps -ef | grep kong | grep -v grep

# Verificar a versão
kong version

# Validar a configuração
kong check /etc/kong/kong.conf

# Verificar a saúde do Kong
kong health

# Verificar o status via systemd (caso aplicável)
systemctl status kong --no-pager

# Verificar portas abertas
ss -lntp | grep -E '8000|8001|8443|8444'

# Verificar memória
free -h

# Verificar disco
df -h

# Visualizar os últimos erros do Kong
tail -100 /usr/local/kong/logs/error.log

# Acompanhar erros em tempo real
tail -f /usr/local/kong/logs/error.log

# Visualizar access log
tail -100 /usr/local/kong/logs/access.log

# Acompanhar access log em tempo real
tail -f /usr/local/kong/logs/access.log

# Procurar requisições com HTTP 502
grep ' 502 ' /usr/local/kong/logs/access.log | tail -20

# Procurar um request específico no error.log
grep "<KONG_REQUEST_ID>" /usr/local/kong/logs/error.log

# Procurar erros de um horário específico
grep "15:41" /usr/local/kong/logs/error.log

# Procurar erros críticos
grep -Ei "error|crit|alert|panic|fail|timeout" /usr/local/kong/logs/error.log | tail -100

# Consultar a Admin API
curl -s http://localhost:8001/

# Listar Services
curl -s http://localhost:8001/services

# Listar Routes
curl -s http://localhost:8001/routes

# Listar Upstreams
curl -s http://localhost:8001/upstreams

# Verificar a saúde de um Upstream
curl -s http://localhost:8001/upstreams/<UPSTREAM>/health

# Listar plugins habilitados
curl -s http://localhost:8001/plugins/enabled

# Obter informações da instância Kong
curl -s http://localhost:8001/

# Testar conectividade com o backend (HTTP)
curl -vk http://<HOST>:<PORT>

# Testar conectividade com o backend (HTTPS)
curl -vk https://<HOST>:<PORT>

# Testar um endpoint específico do backend
curl -vk http://<HOST>:<PORT>/<ENDPOINT>

# Resolver DNS
nslookup <HOST>

# Resolver DNS (alternativa)
dig <HOST>

# Testar conectividade TCP
nc -zv <HOST> <PORT>

# Alternativa ao nc
telnet <HOST> <PORT>

# Verificar consumo dos processos nginx
ps -o pid,%cpu,%mem,cmd -C nginx

# Verificar utilização por thread
top -Hp $(pidof nginx)


curl -s http://localhost:8001/services

grep "7056dd7d7b02d212bd2bef4d5e86426c" /usr/local/kong/logs/error.log

grep "38fe910f11759a18182d606d12d3f559" /usr/local/kong/logs/error.log

grep "9208cf32730eaa5bcb324878f401da4c" /usr/local/kong/logs/error.log