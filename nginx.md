# NGINX 

# Sumário Interativo

- [NGINX](#nginx)
  - [O que é o NGINX?](#o-que-é-o-nginx)
  - [Função do NGINX](#função-do-nginx)
  - [Diferença entre NGINX e Apache](#diferença-entre-nginx-e-apache)
  - [Por que o NGINX é rápido?](#por-que-o-nginx-é-rápido)
- [Instalando o NGINX](#instalando-o-nginx)
  - [1. Verificar se o NGINX está instalado](#1-verificar-se-o-nginx-está-instalado)
  - [2. Instalar o NGINX](#2-instalar-o-nginx)
  - [3. Verificar se o serviço do NGINX está rodando](#3-verificar-se-o-serviço-do-nginx-está-rodando)
  - [4. Testar no navegador](#4-testar-no-navegador)
- [Como o NGINX e o Computador Sabem o que é "localhost"?](#como-o-nginx-e-o-computador-sabem-o-que-é-localhost)
  - [O que é "localhost"?](#o-que-é-localhost)
  - [Como o computador entende isso?](#como-o-computador-entende-isso)
  - [Onde fica o arquivo de hosts?](#onde-fica-o-arquivo-de-hosts)
  - [O que tem nesse arquivo?](#o-que-tem-nesse-arquivo)
  - [Conclusão](#conclusão)
- [Configuração Inicial do NGINX](#configuração-inicial-do-nginx)
  - [Como obter informações sobre o NGINX](#como-obter-informações-sobre-o-nginx)
- [Analisando o Arquivo Principal de Configuração: nginx.conf](#analisando-o-arquivo-principal-de-configuração-nginxconf)
  - [Estrutura do nginx.conf](#estrutura-do-nginxconf)
  - [Configurações de Servidor HTTP no NGINX](#configurações-de-servidor-http-no-nginx)
- [Boas práticas de configuração](#boas-práticas-de-configuração)
- [Configurando um Novo Servidor no NGINX do Zero](#configurando-um-novo-servidor-no-nginx-do-zero)
  - [Onde criar a nova configuração](#onde-criar-a-nova-configuração)
  - [Passos para configurar seu próprio servidor](#passos-para-configurar-seu-próprio-servidor)
- [Configuração de Logs no NGINX no Ubuntu](#configuração-de-logs-no-nginx-no-ubuntu)
  - [1. Diretório para Logs](#1-diretório-para-logs)
  - [2. Configure o Formato do Log no NGINX](#2-configure-o-formato-do-log-no-nginx)
  - [3. Configure o access_log no site específico](#3-configure-o-access_log-no-site-específico)
  - [4. Teste e recarregue a configuração do NGINX](#4-teste-e-recarregue-a-configuração-do-nginx)
  - [5. Visualize os Logs em tempo real](#5-visualize-os-logs-em-tempo-real)
- [Personalização do Formato dos Logs no NGINX](#personalização-do-formato-dos-logs-no-nginx)
  - [1. Edite o arquivo principal do NGINX](#1-edite-o-arquivo-principal-do-nginx)
  - [2. Aplique o formato no access_log do site](#2-aplique-o-formato-no-access_log-do-site)
  - [3. Teste a sintaxe e recarregue o NGINX](#3-teste-a-sintaxe-e-recarregue-o-nginx)
  - [4. Verifique os logs](#4-verifique-os-logs)
- [Adicionando Informações com Cabeçalhos Personalizados no NGINX](#adicionando-informações-com-cabeçalhos-personalizados-no-nginx)


## O que é o NGINX?

* É um **servidor web**.
* Serve para **receber requisições** (HTTP) e **responder** com arquivos ou **encaminhar** para uma aplicação (PHP, Java, etc).
* Funciona na **arquitetura cliente-servidor**: cliente (navegador) faz um pedido → servidor (NGINX) responde.

## Função do NGINX

* **Ouve uma porta** (normalmente a **porta 80** no HTTP).
* Quando chega uma requisição:

  * Se for um **arquivo estático** (imagem, CSS, HTML), ele **devolve diretamente**.
  * Se for algo dinâmico (por exemplo, uma rota de aplicação), ele **repassa para um servidor de aplicação** (como Tomcat, PHP-FPM, etc).

## Diferença entre NGINX e Apache

* **Apache**: cria um novo processo para cada requisição → mais pesado.
* **NGINX**: usa **programação assíncrona**:

  * Cria **um processo principal** (patrão).
  * Cria **processos trabalhadores (workers)** — normalmente um para cada núcleo da CPU.
  * Cada worker trata **várias requisições ao mesmo tempo** (Multiplexing I/O).

## Por que o NGINX é rápido?

* Ele **não espera** terminar uma requisição para pegar outra.
* Enquanto espera a resposta de uma, **já trabalha em outra**.
* Resultado: **alta performance** e **uso eficiente de recursos**.

---

# Instalando o NGINX 

### 1. Verificar se o NGINX está instalado

Abra o terminal e digite:

```bash
nginx -v
```

* Se aparecer a versão do NGINX, ele **já está instalado**.
* Se aparecer "comando não encontrado", **não está instalado**.

### 2. Instalar o NGINX

Atualize a lista de pacotes:

```bash
sudo apt update
```

Instale o NGINX:

```bash
sudo apt install nginx
```

### 3. Verificar se o serviço do NGINX está rodando

Após a instalação, o NGINX geralmente já inicia automaticamente. Para confirmar:

```bash
sudo systemctl status nginx
```

* Se aparecer "active (running)", o NGINX está funcionando.

**Se não estiver ativo**, inicie com:

```bash
sudo systemctl start nginx
```

**Se quiser que o NGINX sempre inicie automaticamente com o sistema:**

```bash
sudo systemctl enable nginx
```

### 4. Testar no navegador

Abra o navegador e digite:

```
http://localhost
```

* Se aparecer a **página padrão do NGINX**, tudo está certo!

![alt text](images/image.png)
---

# Como o NGINX e o Computador Sabem o que é "localhost"?

## O que é "localhost"?

* **"localhost"** é um **nome** usado para se referir **à própria máquina** onde você está.
* Quando você acessa `http://localhost`, na verdade está acessando seu próprio computador.

## Como o computador entende isso?

* Todo sistema operacional tem um **arquivo de hosts**.
* Esse arquivo **faz a ligação** entre **nomes** (como "localhost") e **endereços IP**.

## Onde fica o arquivo de hosts?

* **Linux**: `/etc/hosts`
* **MacOS**: `/private/etc/hosts`
* **Windows**: `C:\Windows\System32\Drivers\Etc\hosts`

## O que tem nesse arquivo?

* Dentro desse arquivo, existe uma linha como esta:

  ```
  127.0.0.1    localhost
  ```

* Isso significa: **sempre que alguém tentar acessar "localhost", o sistema vai redirecionar para o IP 127.0.0.1** (que é o IP da própria máquina).

## Conclusão

* Quando você digita `http://localhost` no navegador:

  * O sistema olha no arquivo de hosts.
  * Vê que "localhost" aponta para `127.0.0.1`.
  * Manda a requisição para `127.0.0.1`, onde o NGINX está rodando e responde com a página HTML padrão.


---

# Configuração Inicial do NGINX

## Como obter informações sobre o NGINX

* No terminal, rodamos:

```bash
nginx -h
```

* Esse comando mostra:

  * A **versão** instalada.
  * **Opções de uso** disponíveis.
  * **Caminhos importantes**: onde estão as configurações, logs, e arquivos HTML.

![alt text](images/image2.png)

### Atenção:

* Cada sistema pode ter caminhos diferentes.

---

# Analisando o Arquivo Principal de Configuração: `nginx.conf`

## Estrutura do `nginx.conf`

Usamos o comando a baixo para editar o arquivo principal de configuração do NGINX.

```bash
vim /etc/nginx/nginx.conf
```

* Linhas que começam com `#` são **comentários** (não são executadas).
* Principais pontos do arquivo:

  * **worker\_processes**:

    * Define quantos processos trabalhadores o NGINX vai criar.
    * `1` significa um processo só. Se configurado como `auto`, ele cria um worker por núcleo de CPU.

  * **worker\_connections**:

    * Número máximo de **conexões simultâneas** por worker.
    * No Linux, tudo é tratado como arquivo: conexões, sockets, pastas etc.

---

## Configurações de Servidor HTTP no NGINX

* **listen**:

  * Define qual **porta** o servidor vai ouvir (exemplo: `8080`).
* **server\_name**:

  * Define qual **nome de servidor** será aceito (exemplo: `localhost`).

> Em servidores reais, configuramos aqui o **nome de domínio** (ex.: `meusite.com`).

* **location /**:

  * Define o que fazer quando o navegador pedir `/` (raiz).
  * Indica que deve buscar o arquivo padrão `index.html` ou `index.htm` em uma **pasta HTML** definida no caminho padrão.


**Resumo Visual:**

```
nginx.conf
│
├── worker_processes
├── worker_connections
├── http {
│    ├── server {
│         ├── listen (porta)
│         ├── server_name (nome)
│         ├── location / (busca index.html)
│    }
│ }
```

## Testando a página padrão

1. Localize a pasta HTML (no exemplo: `/usr/local/Cellar/nginx/html/`).
2. Edite o arquivo `index.html`.
3. Faça alterações, salve e atualize o navegador para ver as mudanças.

![alt text](images/image3.png)

---

# Boas práticas de configuração

* **Evitar editar diretamente** o `nginx.conf` principal para criar novos servidores.
* Para novos sites ou servidores:

  * Usar a pasta `servers` (ou `sites-available`/`sites-enabled` em distribuições Linux).
  * Criar **arquivos de configuração separados** para cada servidor (chamados de "virtual hosts").


---
Claro! Aqui está o conteúdo da **apostila** corrigido conforme o **seu ambiente real**, onde o NGINX usa as pastas **`sites-available/`** e **`sites-enabled/`**, e não `servers/`.
Organizei tudo no mesmo estilo claro e didático que estamos seguindo:

---

# Configurando um Novo Servidor no NGINX do Zero

## Onde criar a nova configuração

* Em distribuições como Ubuntu, o NGINX usa duas pastas principais:

  * `/etc/nginx/sites-available/`: onde **criamos** arquivos de configuração.
  * `/etc/nginx/sites-enabled/`: onde **ativamos** os sites usando **links simbólicos**.

* O arquivo principal `nginx.conf` **inclui** os arquivos ativos de `sites-enabled`.

---

# Passos para configurar seu próprio servidor


**1. Instale o NGINX**

```bash
sudo apt update
sudo apt install nginx
```

**2. Crie um diretório para o seu site**

Por padrão, os arquivos ficam em `/var/www/html`, mas você pode criar outro diretório se quiser, por exemplo:

```bash
sudo mkdir -p /var/www/meusite
sudo chown -R $USER:$USER /var/www/meusite
```

**3. Crie sua página HTML**

```bash
nano /var/www/meusite/index.html
```

Exemplo de conteúdo:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Meu Site NGINX</title>
</head>
<body>
    <h1>Servidor Web com NGINX no Ubuntu</h1>
    <p>Funcionando!</p>
</body>
</html>
```

Salve e feche o arquivo (Ctrl+O, Enter, depois Ctrl+X).

**4. Crie um arquivo de configuração para o seu site**

```bash
sudo nano /etc/nginx/sites-available/meusite
```

Cole o seguinte conteúdo (ajuste `server_name` se quiser usar um domínio):

```nginx
server {
    listen 80;
    server_name localhost;

    root /var/www/meusite;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

Salve e feche.

**5. Ative a configuração do site**

Crie um link simbólico em `sites-enabled`:

```bash
sudo ln -s /etc/nginx/sites-available/meusite /etc/nginx/sites-enabled/
```

**6. Remova a configuração padrão se não precisar**

```bash
sudo rm /etc/nginx/sites-enabled/default
```

(Se não existir, ignore o erro.)

**7. Teste a configuração**

```bash
sudo nginx -t
```

Se mostrar "syntax is ok", prossiga.

**8. Reinicie o NGINX**

```bash
sudo systemctl restart nginx
```

**9. Acesse seu site**

Abra o navegador e acesse `http://localhost` ou o IP da máquina.

### Verifique se o NGINX está rodando

```bash
sudo systemctl status nginx
```

Se aparecer "inactive" ou "failed", inicie o serviço:

```bash
sudo systemctl start nginx
```
---

### Correção e Adaptação do Passo a Passo de Configuração dos Logs para seu servidor NGINX no Ubuntu

Abaixo segue o passo a passo ajustado para o servidor que você configurou no Ubuntu (considerando o diretório `/var/www/meusite`):

---

# Configuração de Logs no NGINX no Ubuntu

### 1. Diretório para Logs

Crie o diretório específico para armazenar os logs do seu site com permissões corretas:

```bash
sudo mkdir -p /var/log/nginx/meusite
sudo chown -R www-data:www-data /var/log/nginx/meusite
```

---

### 2. Configure o Formato do Log no NGINX

Edite o arquivo principal do NGINX para habilitar um formato padrão de logs:

```bash
sudo nano /etc/nginx/nginx.conf
```

Verifique ou adicione a linha abaixo dentro do bloco `http {}`:

```nginx
http {
    log_format main '$remote_addr - $remote_user [$time_local] "$request" '
                    '$status $body_bytes_sent "$http_referer" '
                    '"$http_user_agent" "$http_x_forwarded_for"';
}
```

Salve (`Ctrl + O`) e feche (`Ctrl + X`).

---

### 3. Configure o `access_log` no site específico

Edite a configuração do seu site já existente (`meusite`):

```bash
sudo nano /etc/nginx/sites-available/meusite
```

Modifique o conteúdo adicionando as linhas referentes ao `access_log` e ao `error_log`:

```nginx
server {
    listen 80;
    server_name localhost;

    root /var/www/meusite;
    index index.html;

    access_log /var/log/nginx/meusite/access.log main;
    error_log /var/log/nginx/meusite/error.log;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

Salve e feche o arquivo.

---

### 4. Teste e recarregue a configuração do NGINX

Verifique a sintaxe da sua configuração:

```bash
sudo nginx -t
```

Se estiver correto, recarregue as configurações:

```bash
sudo systemctl reload nginx
```

---

### 5. Visualize os Logs em tempo real

Agora, você pode monitorar os logs do seu site assim:

```bash
tail -f /var/log/nginx/meusite/access.log
```

E para os erros:

```bash
tail -f /var/log/nginx/meusite/error.log
```

---

### Observação Importante: IPs reais com Proxy Reverso

Se futuramente você usar o NGINX como proxy reverso, adicione o cabeçalho `X-Forwarded-For` para capturar o IP real do usuário original:

```nginx
location / {
    proxy_pass http://IP_SERVICO_INTERNO;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
}
```

E ajuste o formato do log para capturar o cabeçalho `X-Forwarded-For`, como já mostrado acima.

---

### Personalização do Formato dos Logs no NGINX

Agora que os logs estão habilitados e funcionando, o próximo passo é **personalizar o formato** desses registros para torná-los mais **claros, enxutos e úteis** durante a análise. O NGINX permite isso por meio da diretiva `log_format`.

---

# Formatando os logs 

### 1. Edite o arquivo principal do NGINX

Abra o arquivo `nginx.conf` para definir um formato de log personalizado:

```bash
sudo nano /etc/nginx/nginx.conf
```

Localize o bloco `http {}` e defina ou edite a diretiva `log_format` com uma estrutura mais limpa, como abaixo:

```nginx
http {
    log_format main 'Remote Addr: $remote_addr, '
                    'Time: [$time_local], '
                    'Request: "$request", '
                    'Status: $status;';
    
    # outras diretivas...
}
```

### Explicando os campos usados:

* `$remote_addr`: IP de quem fez a requisição
* `$time_local`: Data e hora da requisição
* `$request`: Método, URL e versão HTTP
* `$status`: Código de status HTTP da resposta

### Campos Removidos:

* `$remote_user`: Geralmente não é utilizado e fica sempre vazio
* `$body_bytes_sent`: Pouco útil em muitos casos simples
* `$http_user_agent`, `$http_referer`, `$http_x_forwarded_for`: Removidos para simplificar visualmente os logs

---

### 2. Aplique o formato no `access_log` do site

No arquivo do seu site, já criado em:

```bash
sudo nano /etc/nginx/sites-available/meusite
```

Inclua o `main` como nome do formato no `access_log`:

```nginx
access_log /var/log/nginx/meusite/access.log main;
```

Se você já tiver essa linha, apenas adicione `main` ao final, como acima.

---

### 3. Teste a sintaxe e recarregue o NGINX

Verifique se não há erros de configuração:

```bash
sudo nginx -t
```

Se tudo estiver correto, recarregue o NGINX:

```bash
sudo systemctl reload nginx
```

---

### 4. Verifique os logs

Com o novo formato em vigor, você pode visualizar os logs com:

```bash
tail -f /var/log/nginx/meusite/access.log
```

Você verá entradas no formato:

```
Remote Addr: 127.0.0.1, Time: [17/May/2025:15:10:12 -0300], Request: "GET / HTTP/1.1", Status: 200;
```

---

### Adicionando Informações com Cabeçalhos Personalizados no NGINX

Quando usamos **NGINX como load balancer ou proxy reverso**, é comum perdermos informações importantes sobre quem fez a requisição original, já que o IP registrado nos logs dos microsserviços será o do próprio proxy (e não do cliente real). Para resolver esse problema, podemos **adicionar cabeçalhos HTTP personalizados** que carreguem essas informações até os serviços de destino.


[🔝 Voltar ao topo](#sumário-interativo)

---

