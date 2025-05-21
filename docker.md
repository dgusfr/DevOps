# Contêineres e Docker

Contêineres são uma tecnologia de virtualização que permite empacotar uma aplicação e todas as suas dependências em uma unidade isolada, garantindo que ela funcione de maneira consistente em diferentes ambientes.

Isso resolve problemas comuns de incompatibilidade entre sistemas operacionais, bibliotecas ou configurações de ambiente.

O **Docker** é uma plataforma que utiliza contêineres para facilitar o desenvolvimento, implantação e execução de aplicações. Ele simplifica a criação, distribuição e gerenciamento de contêineres, proporcionando agilidade e consistência em todo o ciclo de vida do software.

![ambientes-dev-test-prod](images/ambientes-dev-test-prod.png)

---

## O Processo de Desenvolvimento

O desenvolvimento de software segue um fluxo típico:

- **Identificação de Necessidades**: Definição dos requisitos funcionais e não funcionais da aplicação.
- **Especificação Técnica**: Documentação detalhada do que será desenvolvido.
- **Documentação e Desenvolvimento**: Implementação das funcionalidades seguindo as especificações.
- **Testes e Revisão**: Validação das funcionalidades para garantir qualidade.
- **Homologação e Produção**: Publicação da solução para que os usuários possam acessá-la.

![fluxo-processo-desenvolvimento](images/fluxo-processo-desenvolvimento.png)

Esse processo envolve transições entre diferentes ambientes (desenvolvimento, teste e produção), onde dependências e bibliotecas podem variar, gerando problemas de compatibilidade.

---

## Desafios na Transição Entre Ambientes

O principal desafio na transição entre ambientes é a **inconsistência causada por diferenças em**:

- Versões de bibliotecas.
- Configurações do sistema.
- Sistemas operacionais.

Um software pode funcionar no ambiente de desenvolvimento, mas falhar no de produção devido a essas diferenças.  
É essencial garantir que o ambiente seja **reproduzível e consistente** em todas as fases do ciclo de vida do software.

---

## Solução com Contêineres

Os contêineres encapsulam o software e suas dependências, garantindo que ele funcione da mesma forma em qualquer ambiente. Eles fornecem:

- **Isolamento**: Cada contêiner funciona de forma independente, como se fosse uma aplicação em sua própria máquina.
- **Eficiência**: Não é necessário executar sistemas operacionais completos, reduzindo o consumo de recursos.
- **Reprodutibilidade**: Asseguram que a aplicação tenha o mesmo comportamento em desenvolvimento, teste e produção.

---

## Diferenças Entre Máquinas Virtuais e Contêineres

### Máquinas Virtuais (VMs):

- Utilizam um **hypervisor** para criar ambientes virtuais.
- Cada VM executa um **sistema operacional completo**, consumindo mais recursos.
- Oferecem maior isolamento, mas são menos eficientes para execução de aplicações leves.

### Contêineres:

- Utilizam o **kernel do sistema operacional hospedeiro**, sem precisar de um hypervisor.
- Compartilham o sistema operacional, consumindo **menos recursos**.
- São mais **leves e rápidos**, ideais para escalar aplicações.

### Comparação Estrutural:

![comparacao-container-vm](images/comparacao-container-vm.png)

---

## Mecanismos de Isolamento dos Contêineres

Os contêineres utilizam **namespaces** e **cgroups** para isolar processos e gerenciar recursos.

### Namespaces

Isolam diferentes aspectos dos contêineres:

- **PID**: Isolamento dos processos.
- **NET**: Isolamento de redes e interfaces.
- **IPC**: Isolamento da comunicação entre processos.
- **MNT**: Isolamento do sistema de arquivos.
- **UTS**: Permite que o contêiner tenha seu próprio nome de host.

### Cgroups

Controlam o uso de recursos (CPU, memória, I/O) de cada contêiner.

Esses mecanismos permitem que os contêineres funcionem como **processos isolados dentro de uma máquina**, proporcionando **segurança e eficiência**.

---

Aqui está o conteúdo convertido para **Markdown**, seguindo o padrão da sua apostila:

---

# Instalação do Docker no Linux

## 1. Pré-requisitos

- **Sistema Operacional**: Distribuições baseadas em Linux como Ubuntu, Debian, Fedora, CentOS, etc.
- **Acesso de Superusuário**: Permissões para executar comandos com `sudo`.

---

## 2. Atualizar o Sistema

```bash
sudo apt-get update
sudo apt-get upgrade
````

---

## 3. Instalar Dependências

```bash
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```

---

## 4. Adicionar a Chave GPG Oficial do Docker

```bash
sudo mkdir -p /etc/apt/keyrings

curl -fsSL https://download.docker.com/linux/ubuntu/gpg \
    | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

---

## 5. Adicionar o Repositório do Docker

```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" \
  | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

---

## 6. Atualizar os Pacotes e Instalar o Docker Engine

```bash
sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

---

## 7. Verificar a Instalação

```bash
sudo docker run hello-world
```

Esse comando baixa e executa uma imagem de teste para confirmar se o Docker está instalado corretamente.

---

## 8. Usar Docker sem `sudo` (opcional)

Para não precisar usar `sudo` a cada comando Docker, adicione seu usuário ao grupo `docker`.

### Adicione seu usuário ao grupo `docker`:

Substitua `<seu-usuario>` pelo seu nome de usuário, ou use `$USER`:

```bash
sudo usermod -aG docker <seu-usuario>
```

Ou:

```bash
sudo usermod -aG docker $USER
```

---

### Refaça o login ou ative a sessão com `newgrp`:

Você pode reiniciar a sessão, ou aplicar imediatamente:

```bash
newgrp docker
```

---

### Teste o Docker sem `sudo`:

```bash
docker ps
```

Se funcionar sem erro, a configuração está correta.

---

## ⚠️ Outras distribuições

Se estiver utilizando uma distribuição diferente do Ubuntu, consulte a documentação oficial do Docker:

[https://docs.docker.com/engine/install/](https://docs.docker.com/engine/install/)

---

# Imagem

No contexto do Docker, uma **imagem** é um arquivo que contém tudo o que é necessário para executar um contêiner: código da aplicação, bibliotecas, dependências, variáveis de ambiente e partes do sistema operacional.  
Em resumo, a imagem é um **modelo pronto** que define como o contêiner será criado e executado.

As **imagens são estáticas** — uma “fotografia” do ambiente. Ao criar um contêiner a partir de uma imagem, ele se torna um ambiente dinâmico e isolado no qual os processos podem ser executados.

---

## `docker run`

O comando `docker run` cria **e** executa um contêiner a partir de uma imagem. Etapas internas:

1. **Busca pela imagem**  
   - Se a imagem não existir localmente, o Docker faz download (pull) do Docker Hub ou repositório configurado.
2. **Criação do contêiner**  
   - A imagem é instanciada em um contêiner.
3. **Execução**  
   - O contêiner inicia e roda o comando definido (na imagem ou via CLI).

Exemplo:

```bash
docker run hello-world
````

![dockerhub-hello-world](images/dockerhub-hello-world.png)

O Docker cria um contêiner a partir da imagem `hello-world` e exibe uma mensagem confirmando a instalação.

---

## Principais comandos

| Comando                      | Descrição                                                                       |
| ---------------------------- | ------------------------------------------------------------------------------- |
| `docker run <nome> sleep 1d` | Cria um contêiner (ou usa imagem já presente) e executa `sleep 1d` dentro dele. |
| `docker stop <nome>`         | Envia **SIGTERM** para parar o contêiner de forma controlada.                   |
| `docker start <nome>`        | Inicia um contêiner parado anteriormente.                                       |

---

## Subcomandos úteis do `docker run`

* `--name` / `-–name` — Define um nome amigável para o contêiner.
* `-d` — Executa em **segundo plano** (detached).
* `-it` — Modo interativo com terminal.
* `-p` — Mapeia portas (host\:container).
* `-v` — Monta **volumes** host ↔ contêiner.
* `--rm` — Remove o contêiner após saída.
* `-e`, `--env` — Define variáveis de ambiente.
* `--network` — Conecta o contêiner a uma rede Docker.
* `--cpus` — Limita CPU.
* `--memory` — Limita memória.
* `--restart` — Políticas de reinício automático.
* `--detach-keys` — Combinação de teclas para sair sem parar o contêiner.
* `--entrypoint` — Sobrescreve o comando de entrada.
* `--log-driver` — Define driver de logs.
* `--add-host` — Adiciona entradas ao `/etc/hosts` do contêiner.
* `--link` — Liga contêineres para comunicação direta.
* `--privileged` — Acesso privilegiado ao host.
* `--user` — Usuário que executa o comando dentro do contêiner.
* `--workdir` — Define diretório de trabalho no contêiner.
* `--health-cmd` — Comando de verificação de saúde.

---

# Docker Hub

O **Docker Hub** é o repositório oficial de imagens Docker, usado como fonte padrão para pulls.

* Hospeda **imagens oficiais** (ex.: `nginx`, `mysql`, `ubuntu`).
* Permite que usuários **publiquem** suas próprias imagens.
* O Docker procura imagens aqui por padrão.

### Exemplo de uso

1. Pesquise a imagem **Ubuntu**: [https://hub.docker.com](https://hub.docker.com)
2. Baixe a imagem:

```bash
docker pull ubuntu
```

Isso faz o download da versão mais recente da imagem **Ubuntu**.


---

# Criando Contêineres

Ao criar um contêiner a partir de uma imagem, você executa aplicações ou processos de forma isolada.

- Para criar **e** executar um contêiner com a imagem Ubuntu:

```bash
docker run ubuntu
````

O Docker verifica se a imagem está disponível localmente; se não, faz download do Docker Hub e cria o contêiner.

* Para **baixar** a imagem sem executá-la:

```bash
docker pull ubuntu
```

---

## Verificando Contêineres

| Comando        | Descrição                                              |
| -------------- | ------------------------------------------------------ |
| `docker ps`    | Lista apenas contêineres em execução.                  |
| `docker ps -a` | Lista **todos** os contêineres (ativos e finalizados). |

A saída inclui `CONTAINER ID`, `IMAGE`, `STATUS`, etc.

---

### Exemplo prático — Ubuntu

1. Execute:

   ```bash
   docker run ubuntu
   ```

2. Liste os contêineres:

   ```bash
   docker ps -a
   ```

O contêiner aparecerá com `STATUS` **Exited**, porque nenhum processo contínuo estava em execução.

---

# Interagindo com Contêineres

## Passo a passo — subindo a imagem Ubuntu

1. **Baixar a imagem**

   ```bash
   docker pull ubuntu
   ```

2. **Executar um contêiner em segundo plano**

   ```bash
   docker run --name meu_ubuntu -d ubuntu sleep 1d
   ```

3. **Verificar contêineres em execução**

   ```bash
   docker ps
   ```

4. **Acessar o terminal do contêiner**

   ```bash
   docker exec -it meu_ubuntu bash
   ```

   * `-it` → modo interativo
   * `bash` → abre shell dentro do contêiner

   Exemplos internos:

   ```bash
   ls -a            # listar arquivos
   touch arquivo_teste
   ```

   Saia com **`Ctrl + D`**.

5. **Parar o contêiner**

   ```bash
   docker stop meu_ubuntu            # parada controlada (SIGTERM)
   docker stop -t=0 meu_ubuntu       # parada imediata
   ```

6. **Reiniciar o contêiner**

   ```bash
   docker start meu_ubuntu
   ```

7. **Pausar / retomar processos**

   ```bash
   docker pause meu_ubuntu
   docker unpause meu_ubuntu
   ```

8. **Remover o contêiner**

   ```bash
   docker rm meu_ubuntu            # contêiner parado
   docker rm --force meu_ubuntu    # contêiner em execução
   ```

---

## Persistência e Isolamento

* **Isolamento**: arquivos dentro do contêiner não aparecem no host.
* **Persistência**: dados desaparecem quando o contêiner é removido, exceto se **volumes** ou **bind mounts** forem configurados.

---

## Tabela de comandos-chave

| Comando                      | Função                        |
| ---------------------------- | ----------------------------- |
| `docker exec -it <ctr> bash` | Abre shell interativo.        |
| `docker stop <ctr>`          | Para de forma controlada.     |
| `docker stop -t=0 <ctr>`     | Para imediatamente.           |
| `docker start <ctr>`         | Reinicia contêiner parado.    |
| `docker pause <ctr>`         | Pausa processos.              |
| `docker unpause <ctr>`       | Retoma processos.             |
| `docker rm <ctr>`            | Remove contêiner parado.      |
| `docker rm --force <ctr>`    | Remove contêiner em execução. |

---

## 📘 Praticando: Criando Primeira Imagem  com Docker


Você é responsável por empacotar uma aplicação web estática (um site simples em HTML) utilizando Docker. O objetivo é garantir que essa aplicação funcione exatamente igual em qualquer máquina.

Para isso, você deverá:

1. Criar um diretório de projeto com uma página HTML.
2. Criar um **Dockerfile** que usa a imagem oficial `nginx` como base.
3. Copiar o conteúdo da página HTML para dentro da imagem.
4. Gerar uma imagem com o nome `meu-nginx-lab` e a tag `1.0`.
5. Executar um contêiner a partir dessa imagem com:
   - Um nome amigável (`web-lab`)
   - Porta 8080 mapeada para a porta 80 do contêiner
   - Montagem de volume em modo somente leitura
   - Execução em segundo plano (`-d`)
6. Verificar o funcionamento no navegador (`http://localhost:8080`) ou com `curl`.
7. Executar comandos extras como `docker ps`, `docker exec`, `docker logs` e `docker stop`.


### 🎯 Requisitos técnicos

- Utilizar o comando `docker build` para criar a imagem.
- Utilizar `docker run` com os parâmetros corretos.
- Utilizar `volumes` e `portas`.
- Usar o `nginx` como servidor HTTP.



### ✅ Entrega esperada

- Um contêiner rodando sua aplicação em `http://localhost:8080`.
- O código-fonte do seu site visível dentro do contêiner em `/usr/share/nginx/html`.
- A imagem criada localmente com o nome `meu-nginx-lab:1.0`.



# Resolução Proposta


## 1. Criar a estrutura de diretórios

```bash
mkdir -p ~/meu-docker-lab/app
cd ~/meu-docker-lab
````


```bash
cat > app/index.html <<'EOF'
<!DOCTYPE html>
<html>
  <head>
    <title>Meu Docker Lab</title>
    <meta charset="utf-8">
  </head>
  <body>
    <h1>Funcionando no contêiner!</h1>
  </body>
</html>
EOF
```



## 3. Criar o `Dockerfile`

```bash
cat > Dockerfile <<'EOF'
FROM nginx:alpine                
COPY app/ /usr/share/nginx/html  
EXPOSE 80                       
EOF
```

## 4. Construir a imagem

```bash
docker build -t meu-nginx-lab:1.0 .
```

> Resultado esperado: *“Successfully tagged meu-nginx-lab:1.0”*


## 5. Executar o contêiner

```bash
docker run -d \
  --name web-lab \
  -p 8080:80 \
  -v ~/meu-docker-lab/app:/usr/share/nginx/html:ro \
  --rm \
  meu-nginx-lab:1.0
```


## 6. Testar

```bash
curl http://localhost:8080
```

Deve retornar:

```html
<h1>Funcionando no contêiner!</h1>
```


## 7. Comandos de inspeção

```bash
docker ps
docker logs web-lab
docker exec -it web-lab sh
```

Saia do shell com `exit`.


## 8. Parar (encerra e remove por causa do --rm)

```bash
docker stop web-lab
```



## 9. Limpar a imagem (opcional)

```bash
docker image rm meu-nginx-lab:1.0
```

---

<br>
<br>
<br>

---

# Acessando Aplicações Web com Docker

No Docker, podemos executar aplicações web em contêineres e acessá-las diretamente pelo navegador.  
Abaixo, detalhamos como criar, gerenciar e acessar essas aplicações utilizando imagens públicas do Docker Hub.


## 1. Executando uma Aplicação Web no Contêiner

### Escolhendo a Imagem

Neste exemplo, usaremos a imagem `docker/example-voting-app-vote`, que já contém uma aplicação web de exemplo.

![dockerhub-voting-app](images/dockerhub-voting-app.png)

### Executando o contêiner:

```bash
docker run -d docker/example-voting-app-vote
```

## 2. Verificando o Contêiner

Após executar o contêiner, verifique se ele está rodando:

```bash
docker ps
```

Saída esperada (exemplo):

![docker-ps-output](images/docker-ps-output.png)


## 3. Mapeando Portas Externas

Ao acessar `localhost:80`, você pode ver erro de conexão. Isso acontece porque as portas do contêiner estão **isoladas** do host por padrão.

### 🔁 Mapeamento Automático de Portas

Execute com `-P`:

```bash
docker run -d -P docker/example-voting-app-vote
```

**O que `-P` faz?**

* Mapeia automaticamente as portas internas do contêiner para portas disponíveis do host.

Verifique com:

```bash
docker ps
```

Exemplo de saída:

```
0.0.0.0:32768->80
```

Acesse a aplicação no navegador:

```
http://localhost:32768
```


### 🎯 Mapeamento Manual de Porta

Você pode definir qual porta usar no host com `-p`:

```bash
docker run -d -p 3000:80 docker/example-voting-app-vote
```

Verifique com:

```bash
docker ps
```

Exemplo de saída:

```
0.0.0.0:3000->80
```

Acesse no navegador:

```
http://localhost:3000
```

![webapp-navegador](images/webapp-navegador.png)

---

## 4. Gerenciamento de Contêineres

### Parar um Contêiner

```bash
docker stop <CONTAINER_ID>
```

### Remover um Contêiner

Primeiro pare:

```bash
docker stop <CONTAINER_ID>
```

Depois remova:

```bash
docker rm <CONTAINER_ID>
```

Se estiver em execução:

```bash
docker rm --force <CONTAINER_ID>
```

### Parar Todos os Contêineres

```bash
docker stop $(docker container ls -q)
```



## 📋 Resumo dos Comandos Usados

| Comando                                 | Função                                               |
| --------------------------------------- | ---------------------------------------------------- |
| `docker run -d <IMAGEM>`                | Executa um contêiner em segundo plano.               |
| `docker run -d -P <IMAGEM>`             | Executa o contêiner e mapeia portas automaticamente. |
| `docker run -d -p <HOST>:<CONTAINER>`   | Mapeia portas manualmente.                           |
| `docker ps`                             | Lista contêineres em execução.                       |
| `docker stop <ID>`                      | Para um contêiner.                                   |
| `docker rm <ID>`                        | Remove um contêiner parado.                          |
| `docker rm --force <ID>`                | Força a remoção de um contêiner em execução.         |
| `docker stop $(docker container ls -q)` | Para todos os contêineres ativos.                    |



Com esses passos, você pode **executar, acessar e gerenciar** aplicações web dentro de contêineres Docker, configurando o mapeamento de portas conforme necessário para acessá-las via navegador.

---

<br>
<br>
<br>

---


# Estrutura de Imagens no Docker

As imagens no Docker são fundamentais para o funcionamento dos contêineres. Elas contêm todos os arquivos, bibliotecas, dependências e configurações necessárias para criar um ambiente isolado. Este conteúdo descreve como as imagens são estruturadas, como interagimos com elas e quais os benefícios dessa estrutura para o uso de contêineres.

## Listando Imagens Localmente

Para listar todas as imagens armazenadas localmente no sistema:

```bash
docker image ls
````

Esse comando exibe uma tabela com as seguintes informações:

* **REPOSITORY**: Nome do repositório da imagem.
* **TAG**: Versão da imagem.
* **IMAGE ID**: Identificador único da imagem.
* **CREATED**: Data de criação da imagem.
* **SIZE**: Tamanho ocupado no disco.

## Camadas das Imagens

As imagens Docker são compostas por múltiplas camadas empilhadas. Essas camadas são:

* Imutáveis (read-only)````markdown
# Estrutura de Imagens no Docker

As imagens no Docker são fundamentais para o funcionamento dos contêineres. Elas contêm todos os arquivos, bibliotecas, dependências e configurações necessárias para criar um ambiente isolado. Este conteúdo descreve como as imagens são estruturadas, como interagimos com elas e quais os benefícios dessa estrutura para o uso de contêineres.

## Listando Imagens Localmente

Para listar todas as imagens armazenadas localmente no sistema:

```bash
docker image ls
````

Esse comando exibe uma tabela com as seguintes informações:

* **REPOSITORY**: Nome do repositório da imagem.
* **TAG**: Versão da imagem.
* **IMAGE ID**: Identificador único da imagem.
* **CREATED**: Data de criação da imagem.
* **SIZE**: Tamanho ocupado no disco.

## Camadas das Imagens

As imagens Docker são compostas por múltiplas camadas empilhadas. Essas camadas são:

* Imutáveis (read-only)
* Compartilhadas entre várias imagens e contêineres
* Otimizadas para reduzir o uso de disco e acelerar operações

## Baixando Imagens do Docker Hub

Para baixar uma imagem pública do Docker Hub, utilize:

```bash
docker pull nginx
```

O download ocorre em etapas, cada uma representando uma camada da imagem. Se uma camada já existir localmente, ela será reutilizada, evitando download desnecessário.

## Visualizando as Camadas de uma Imagem

Para inspecionar o histórico de camadas que compõem uma imagem:

```bash
docker history <IMAGE_ID>
```

Esse comando mostra o histórico de comandos usados para criar a imagem, além do tamanho de cada camada.

![docker-history-camadas](images/docker-history-camadas.png)

## Estrutura Interna de Contêineres

Ao criar um contêiner a partir de uma imagem, ele herda suas camadas de forma imutável. Entretanto, há uma camada adicional que permite alterações:

| Tipo de Camada | Característica                                           |
| -------------- | -------------------------------------------------------- |
| Read-Only      | Camadas herdadas da imagem base, imutáveis               |
| Read-Write     | Criada ao iniciar o contêiner, permite alterações locais |

## Funcionamento Interno

O cliente Docker envia comandos como `docker build`, `docker pull` e `docker run` para o Docker daemon, que gerencia as imagens e contêineres no host.

As imagens armazenadas no host consistem em camadas read-only. Quando um contêiner é iniciado, o Docker adiciona uma camada read-write no topo da imagem base.

![docker-arquitetura-camadas](images/docker-arquitetura-camadas.png)

Fonte: https://www.gta.ufrj.br/ensino/eel879/trabalhos_v1_2017_2/docker/components.html

Essa camada read-write é usada para armazenar quaisquer alterações feitas durante a execução do contêiner, como criação de arquivos ou instalação de pacotes. A imagem base permanece inalterada, o que garante:

* Consistência entre diferentes contêineres
* Reutilização eficiente das camadas read-only
* Isolamento entre contêineres

## Eficiência na Utilização de Camadas

### Compartilhamento de Camadas

Múltiplas imagens e contêineres podem compartilhar camadas read-only comuns, otimizando o uso de espaço em disco.

### Alterações e Persistência

Todas as alterações feitas durante a execução de um contêiner são armazenadas na camada read-write. Quando o contêiner é removido, essas alterações são perdidas, a menos que sejam persistidas por meio de volumes.

## Conclusão

A estrutura de camadas do Docker proporciona:

* Eficiência no armazenamento
* Rapidez na criação de contêineres
* Isolamento entre aplicações
* Reaproveitamento de recursos

Esse modelo é essencial para que o Docker ofereça ambientes portáteis, leves e consistentes em diferentes sistemas e fases do desenvolvimento de software.


* Compartilhadas entre várias imagens e contêineres
* Otimizadas para reduzir o uso de disco e acelerar operações

---

<br>
<br>
<br>

---











