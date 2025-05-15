# NGINX 

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

Aqui está a transcrição que você mandou organizada como **apostila**, seguindo o mesmo padrão didático e direto que estamos mantendo:

---

# Configurando um Novo Servidor no NGINX do Zero

## Onde criar a nova configuração

* O arquivo principal do NGINX (`nginx.conf`) normalmente **inclui** configurações adicionais de outro diretório, como:

  * `/etc/nginx/sites-available/`
  * `/etc/nginx/sites-enabled/`
  * `/usr/local/etc/nginx/servers/`
* Em instalações simples, pode existir um único arquivo chamado **`default.conf`**.

## Passos para configurar

### 1. Localizar a pasta de servidores

* Exemplo de caminho encontrado: `/usr/local/etc/nginx/servers/`
* Verifique com:

```bash
ls /usr/local/etc/nginx/
```

ou usando a tecla `Tab` para auto-completar no terminal.

### 2. Criar o arquivo de configuração

* Crie um novo arquivo (exemplo: `default.conf`):

```bash
sudo vim /usr/local/etc/nginx/servers/default.conf
```

ou substitua o editor por outro de sua preferência (como `nano`).

* Estrutura básica do novo servidor:

```nginx
server {
    listen 80;
    server_name localhost;

    location / {
        root /caminho/absoluto/para/seus/arquivos;
        index index.html;
    }
}
```

### Explicação:

| Diretiva                  | Significado                                                                                           |
| :------------------------ | :---------------------------------------------------------------------------------------------------- |
| `listen 80;`              | O servidor irá ouvir a **porta 80** (HTTP padrão). Se a porta 80 já estiver em uso, altere para 8080. |
| `server_name localhost;`  | O servidor responde quando acessar **localhost** no navegador.                                        |
| `location / {}`           | Instruções para requisições que chegam na raiz (`/`).                                                 |
| `root /caminho/absoluto;` | Pasta onde os **arquivos estáticos** (HTML, imagens, etc.) estarão armazenados.                       |
| `index index.html;`       | Arquivo que será **carregado automaticamente** ao acessar apenas o domínio, sem rota específica.      |

> Importante: use **caminho absoluto** no `root`, começando por `/` no Linux/Mac ou `C:/` no Windows.

---

# Entendendo as Boas Práticas

* **Evitar caminhos com espaços** no nome das pastas.
* **Separar** o diretório dos arquivos do diretório de configuração do NGINX.
* Para cada novo site ou servidor, crie **um novo arquivo de configuração**.

---

# Primeiro Teste no Navegador

* Após salvar o `default.conf`, acesse:

```
http://localhost
```

ou

```
http://localhost:80
```

* Esperado:

  * Se houver um `index.html` correto na pasta indicada, ele será carregado.
  * Se não houver, o navegador exibirá erro (como **404 Not Found**).

> Se der erro de conexão recusada, não significa que a configuração está errada — pode ser necessário **reiniciar o NGINX** ou revisar a ativação do novo servidor.
> Isso será abordado no próximo tópico.

---

**Resumo Visual:**

```
/usr/local/etc/nginx/
│
├── nginx.conf (arquivo principal)
│    └── include servers/*.conf
│
└── servers/
     └── default.conf (nosso novo servidor criado)
```

---

Se quiser, eu já deixo preparado o próximo tópico também (corrigindo o erro de conexão e recarregando o NGINX).
Quer que eu já continue no mesmo formato?


