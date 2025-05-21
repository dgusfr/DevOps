# Bash Scripts

Em ambientes DevOps, a automação de tarefas repetitivas é essencial para aumentar eficiência. Usar scripts Bash permite encapsular comandos em arquivos executáveis, simplificando processos como monitoramento de servidores, gestão de aplicações e rotinas complexas.

---

## Verificando o Status de um Servidor com `curl`

### Instalar o `curl`

```bash
sudo apt install curl
````

### Comando para verificar o status HTTP

```bash
curl --write-out %{http_code} --silent --output /dev/null https://adopet-frontend-cypress.vercel.app/home
```

* O retorno **`200`** indica que a página está funcionando corretamente.

---

## Bash Script: Monitoramento de Servidor

### Passo 1 – Criar o arquivo do script

```bash
nano monitoramento-servidor.sh
```

Conteúdo inicial:

```bash
#!/bin/bash
```

### Passo 2 – Definir variável e executar o comando

```bash
#!/bin/bash

codigo_http=$(curl --write-out %{http_code} --silent --output /dev/null https://adopet-frontend-cypress.vercel.app/home)
echo $codigo_http
```

### Passo 3 – Salvar e sair

No **nano**: `Ctrl + X`, depois `Y`, e `Enter`.

### Gerenciar permissões e executar

```bash
chmod +x monitoramento-servidor.sh
./monitoramento-servidor.sh
```

O script exibirá o código HTTP (ex.: `200`).

---

## Monitorando o Uso de Disco com Script Bash

### 1. Criar o script

```bash
vim monitoramento-disco.sh
```

Shebang:

```bash
#!/bin/bash
```

### 2. Testar comandos manualmente

```bash
df -h                 # uso de disco geral
df -h | grep /mnt/c   # filtrar partição /mnt/c
df -h | grep /mnt/c | awk '{print $5}'   # extrair porcentagem
```

### 3. Script completo

```bash
#!/bin/bash

ponto_montagem="/mnt/c"
uso_disco=$(df -h | grep $ponto_montagem | awk '{print $5}')
echo "Uso de disco em $uso_disco"
```

### 4. Permissão e execução

```bash
chmod +x monitoramento-disco.sh
./monitoramento-disco.sh
```

Saída esperada:

```
Uso de disco em 92%
```

### 5. Organização

* O ponto de montagem é variável (`/mnt/c`) e pode ser alterado conforme necessário.
* O comando `df -h | grep $ponto_montagem | awk '{print $5}'` extrai a coluna **Use%** da partição desejada.

Script final:

```bash
#!/bin/bash

ponto_montagem="/mnt/c"
uso_disco=$(df -h | grep $ponto_montagem | awk '{print $5}')
echo "Uso de disco em $uso_disco"
```

