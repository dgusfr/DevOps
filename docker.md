# Docker

## Sumário Interativo

- [Contêineres e Docker](#contêineres-e-docker)
  - [O Processo de Desenvolvimento](#o-processo-de-desenvolvimento)
  - [Desafios na Transição Entre Ambientes](#desafios-na-transição-entre-ambientes)
  - [Solução com Contêineres](#solução-com-contêineres)
  - [Diferenças Entre Máquinas Virtuais e Contêineres](#diferenças-entre-máquinas-virtuais-e-contêineres)
  - [Mecanismos de Isolamento dos Contêineres](#mecanismos-de-isolamento-dos-contêineres)
- [Instalação do Docker no Linux](#instalação-do-docker-no-linux)
- [Imagem](#imagem)
  - [`docker run`](#docker-run)
  - [Principais Comandos](#principais-comandos)
  - [Subcomandos Úteis do `docker run`](#subcomandos-úteis-do-docker-run)
- [Docker Hub](#docker-hub)
- [Criando Contêineres](#criando-contêineres)
  - [Verificando Contêineres](#verificando-contêineres)
  - [Interagindo com Contêineres](#interagindo-com-contêineres)
- [Praticando: Criando Primeira Imagem com Docker](#praticando-criando-primeira-imagem-com-docker)
- [Acessando Aplicações Web com Docker](#acessando-aplicações-web-com-docker)
- [Estrutura de Imagens no Docker](#estrutura-de-imagens-no-docker)
- [Criando Imagens Docker Personalizadas](#criando-imagens-docker-personalizadas)
- [Persistência de Dados](#persistência-de-dados)
  - [Bind Mount](#bind-mount)
  - [Volumes](#volumes)
  - [TMPFS](#tmpfs)
- [Cloud](#cloud)
  - [Cloud Computing e a AWS](#cloud-computing-e-a-aws)
  - [Usando o Elastic Beanstalk para Implantação](#usando-o-elastic-beanstalk-para-implantação)
  - [Ajustando a Imagem Docker e Subindo na AWS](#ajustando-a-imagem-docker-e-subindo-na-aws)
  - [Subindo a Aplicação na Nuvem](#subindo-a-aplicação-na-nuvem)




---

<br>
<br>
<br>

---

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

[🔝 Voltar ao topo](#sumário-interativo)

---

<br>
<br>
<br>

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

[🔝 Voltar ao topo](#sumário-interativo)

---

<br>
<br>
<br>

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
```

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

[🔝 Voltar ao topo](#sumário-interativo)

---

<br>
<br>
<br>

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

[🔝 Voltar ao topo](#sumário-interativo)

---

<br>
<br>
<br>

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

[🔝 Voltar ao topo](#sumário-interativo)

---

<br>
<br>
<br>

---

## Praticando: Criando Primeira Imagem com Docker


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

[🔝 Voltar ao topo](#sumário-interativo)

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

[🔝 Voltar ao topo](#sumário-interativo)

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

[🔝 Voltar ao topo](#sumário-interativo)

---

<br>
<br>
<br>

---


# Criando Imagens Docker Personalizadas

Para contêinerizar uma aplicação como o front-end do projeto AllBooks, seguimos um processo estruturado que envolve a criação de uma imagem personalizada utilizando um arquivo `Dockerfile`. Abaixo estão as etapas detalhadas.

## 1. Clonando o Repositório do Projeto

Utilize o comando `git clone` para obter os arquivos da aplicação localmente:

```bash
git clone https://github.com/alura-cursos/curso-react-alurabooks/
cd ./curso-react-alurabooks/
```

## 2. Criando o Arquivo Dockerfile

O `Dockerfile` define as instruções para construir a imagem. Crie o arquivo com:

```bash
nano Dockerfile
```

Conteúdo do arquivo:

```dockerfile
FROM node:20
WORKDIR /app
COPY package.json .
RUN npm install
COPY . .
ENTRYPOINT ["npm", "start"]
```

Salve com `Ctrl + X`, `Y`, e `Enter`.

![dockerfile-nano](images/dockerfile-nano.png)

## 3. Construindo a Imagem

Com o `Dockerfile` criado e no diretório correto, execute:

```bash
docker build -t diego-franco/allbooks:1.1 .
```

Este comando executa:

* Download da imagem base (`node:20`)
* Cópia dos arquivos
* Instalação de dependências com `npm install`
* Criação da nova imagem

Após a construção, visualize com:

```bash
docker image ls
```

![docker-build-output](images/docker-build-output.png)

## 4. Criando e Executando o Contêiner

Execute a imagem com o seguinte comando:

```bash
docker run -d -p 8080:3000 diego-franco/allbooks:1.1
```

Explicação:

* `-d`: Executa em segundo plano
* `-p 8080:3000`: Mapeia porta 8080 do host para 3000 do contêiner

Verifique se o contêiner está ativo:

```bash
docker ps
```

![docker-run-ps](images/docker-run-ps.png)

## 5. Testando a Aplicação no Navegador

Abra o navegador e acesse:

```
http://localhost:8080
```

Se a porta estiver corretamente mapeada, a aplicação AllBooks será exibida.

![allbooks-navegador](images/allbooks-navegador.png)

## 6. Publicando a Imagem no Docker Hub

Autentique-se no Docker Hub:

```bash
docker login
```

Publique a imagem:

```bash
docker push diego-franco/allbooks:1.1
```

A imagem agora está disponível publicamente no seu repositório no Docker Hub.

## 7. Inspecionando a Estrutura Interna de uma Imagem

Use o comando abaixo para obter informações detalhadas da imagem:

```bash
docker inspect <IMAGE_ID>
```

Informações exibidas:

* **Id**: Identificador único da imagem
* **RepoTags**: Tags associadas
* **Created**: Data de criação
* **Configurações**: Variáveis de ambiente, entrypoint, etc.
* **Digest**: Hash da imagem

Exemplo de retorno resumido:

```json
{
  "Id": "sha256:fd1d8f58e8aedc22...",
  "RepoTags": ["ubuntu:latest"],
  "Created": "2024-01-25T17:54:41Z",
  "ContainerConfig": {
    "Hostname": "fa818c501516",
    "Cmd": ["/bin/bash"],
    "Env": ["PATH=/usr/local/sbin:/usr/local/bin"]
  }
}
```

[🔝 Voltar ao topo](#sumário-interativo)

---

<br>
<br>
<br>

---

# Persistência de Dados

Persistência de dados é uma funcionalidade essencial em containers, pois permite que informações importantes sejam armazenadas e recuperadas mesmo após a interrupção ou remoção de um container. No Docker, existem três mecanismos principais para persistir dados: **volumes**, **bind mounts** e **tmpfs mounts**. A seguir, detalhamos cada um, com foco inicial no bind mount.

## Bind Mount

O bind mount cria uma ligação direta entre um diretório do sistema de arquivos do host e um diretório específico dentro de um container. Isso permite que dados armazenados no container sejam acessados diretamente pelo host e vice-versa. Esse mecanismo é útil para compartilhar arquivos entre o host e o container.

### Exemplo Prático

**Criar um Diretório no Host**

```bash
mkdir volume-docker
````

**Criar um Container com Bind Mount**

```bash
docker run -it --mount type=bind,source=home/diego-franco/volume-docker,target=/app ubuntu bash
```

* `type=bind`: Define o tipo de montagem como bind mount.
* `source`: Especifica o diretório do host (`/home/alura/volume-docker`).
* `target`: Especifica o diretório no container (`/app`).

**Criar um Arquivo no Container**

```bash
cd /app
touch teste_arquivo
ls
```

O arquivo `teste_arquivo` será exibido dentro do diretório `/app` no container.

**Verificar a Persistência no Host**

Saia do container (Ctrl + D ou `exit`) e verifique o conteúdo no host:

```bash
cd volume-docker
ls
```

O arquivo `teste_arquivo` estará disponível no diretório `volume-docker` no host, confirmando a persistência.

**Recriar o Container**

```bash
docker run -it --mount type=bind,source=/home/diego-franco/volume-docker,target=/app ubuntu bash
```

O arquivo `teste_arquivo` continuará acessível dentro do diretório `/app`.

### Desvantagem do Bind Mount

Embora o bind mount seja simples e eficiente, ele apresenta um ponto fraco: o Docker **não gerencia** o diretório do host. Isso significa que:

* Se o diretório for alterado ou excluído manualmente no host, os dados serão perdidos.
* A segurança e consistência dos dados dependem diretamente do cuidado no gerenciamento do sistema de arquivos do host.

---

## Volumes

Volumes são o mecanismo de persistência de dados mais recomendado pelo Docker, especialmente para ambientes de produção. Ao contrário dos bind mounts, onde a persistência depende de um diretório do sistema de arquivos do host, os volumes utilizam uma área especial dentro do Docker, proporcionando maior segurança e controle.

![alt text](images/volume.png)

### Por que usar volumes?

**Segurança:**
Os dados são armazenados em uma área reservada do Docker, reduzindo a exposição a alterações acidentais ou maliciosas no sistema de arquivos do host.

**Gerenciamento Centralizado:**
O Docker controla todo o ciclo de vida dos volumes, garantindo que os dados sejam gerenciados de forma eficiente.

**Recomendado para Produção:**
Em ambientes onde a integridade dos dados é crítica, volumes são a escolha ideal.

### Criando e Usando Volumes

**1. Listando Volumes Existentes**

```bash
docker volume ls
```

Se nenhum volume foi criado, a lista estará vazia.

**2. Criando um Volume**

```bash
docker volume create novo-volume
```

Verifique novamente com:

```bash
docker volume ls
```

**3. Criando um Container com Volume**

```bash
docker run -it --mount source=novo-volume,target=/app ubuntu bash
```

* `source=novo-volume`: Nome do volume criado.
* `target=/app`: Diretório no container mapeado para o volume.
* `ubuntu`: Imagem usada.
* `bash`: Comando de terminal interativo.

**4. Testando a Persistência**

```bash
cd /app
touch teste_volume
ls
```

O arquivo `teste_volume` será exibido no diretório. Saia do container com Ctrl + D.

**5. Recuperando Dados de um Volume**

```bash
docker run -it --mount source=novo-volume,target=/app ubuntu bash
cd /app
ls
```

O arquivo `teste_volume` estará presente, provando que o volume manteve a persistência dos dados.

**Inspecionando um Volume**

```bash
docker volume inspect novo-volume
```

Esse comando fornece informações detalhadas sobre o volume, como sua localização e configurações.

---

## TMPFS

O TMPFS é um mecanismo de persistência temporária no Docker, ideal para armazenar dados sensíveis como senhas, informações pessoais ou outros dados que não devem ser compartilhados ou persistidos entre containers. Diferente dos volumes e dos bind mounts, o TMPFS armazena os dados **apenas em memória**, e eles desaparecem assim que o container é encerrado.

### Características do TMPFS

* **Armazenamento Temporário:**
  Os dados são armazenados na memória volátil do sistema e não persistem após o término do container.

* **Maior Segurança:**
  Como os dados não são gravados no disco, o TMPFS é indicado para dados sensíveis que precisam ser protegidos contra acessos externos.

* **Dependência do Ambiente:**
  Funciona melhor em ambientes Linux, aproveitando o sistema de arquivos temporários nativo do kernel.

* **Não indicado para Produção de Longo Prazo:**
  Por ser temporário, o TMPFS não deve ser usado para armazenar dados que precisam ser recuperados posteriormente.

### Como Criar um Container com TMPFS

```bash
docker run -it --tmpfs=/app ubuntu bash
```

* `-it`: Cria o container de forma interativa.
* `--tmpfs=/app`: Define o diretório `/app` como temporário.
* `ubuntu`: Imagem utilizada.
* `bash`: Comando executado ao iniciar o container.

**Verificar diretórios temporários**

```bash
ls
```

Diretórios como `/app` e `/tmp` estarão destacados como temporários.

**Criar um arquivo no diretório TMPFS**

```bash
cd /app
touch tesetetmpfs.txt
ls
```

O arquivo estará visível no diretório.

**Saia do container** com `Ctrl + D`.

### Testando a Persistência Temporária

Verifique se o container ainda está ativo:

```bash
docker ps
```

Recrie o container:

```bash
docker run -it --tmpfs=/app ubuntu bash
cd /app
ls
```

O arquivo criado anteriormente (`tesetetmpfs.txt`) **não estará mais presente**, confirmando que os dados armazenados com TMPFS são temporários.

### Por que Usar TMPFS?

O TMPFS é ideal para:

* **Dados Sensíveis**: Como senhas e tokens.
* **Processamento Temporário**: Dados que só precisam existir durante a execução.
* **Ambientes Seguros**: Impede a gravação de dados confidenciais em disco.

O TMPFS complementa os volumes e bind mounts como uma opção leve e segura para armazenamento temporário, oferecendo uma solução eficiente para casos onde a persistência dos dados não é necessária ou é indesejável.

[🔝 Voltar ao topo](#sumário-interativo)

---

<br>
<br>
<br>

---

# Cloud

Para que a aplicação AllBooks funcione de maneira eficiente, segura e escalável, é fundamental compreender a infraestrutura necessária. Isso inclui componentes essenciais como:

- Hospedagem para o front-end, back-end e banco de dados.
- Estratégias de rede para garantir o acesso, como DNS.
- Segurança com firewalls, monitoramento e backups.
- Práticas de DevOps, como integração contínua e gerenciamento da infraestrutura como código.

## Cloud Computing e a AWS

A computação em nuvem, ou cloud computing, é uma solução amplamente adotada para projetos que exigem flexibilidade, escalabilidade e monitoramento. Entre os principais provedores de serviços de nuvem estão:

- AWS (Amazon Web Services)
- Google Cloud
- Microsoft Azure

No caso do AllBooks, utilizaremos o ambiente da AWS para demonstrar como implementar uma aplicação containerizada na nuvem. O serviço específico que será usado é o **Elastic Beanstalk**, que simplifica o processo de implantação e gerenciamento de aplicações na AWS.

## Criando e Configurando uma Conta AWS

**Criar Conta:**

- Acesse o site da AWS e clique em "Crie uma conta AWS".
- Forneça informações como e-mail e cartão de crédito (necessário para acesso à cota gratuita e recursos básicos).

**Acesso ao Console:**

- Após o login, acesse o console AWS, onde os serviços estarão listados.

## Usando o Elastic Beanstalk para Implantação

**Acessar o Elastic Beanstalk:**

- No menu "Serviços", busque por Elastic Beanstalk.
- Clique em "Criar aplicação" para iniciar o processo.

### Etapas de Configuração

**Etapa 1: Configurar o Ambiente**

- Nível do Ambiente: Selecione *Ambiente de Serviço Web* para permitir acesso via web.
- Informações da Aplicação: Nomeie como `"allbooks"`.
- Plataforma: Escolha *Docker*, usando a versão recomendada.
- Mantenha a opção *instância única* para cota gratuita.

**Etapa 2: Configurar o Acesso ao Serviço**

- Escolha um perfil de serviço existente, como `Permission_elastic_beanstalk`.
- Verifique as permissões de criação de instâncias e outras configurações associadas ao IAM.

**Etapa 3: Definir Redes e Sub-redes**

- Configure a VPC (Virtual Private Cloud) padrão e habilite endereços IP públicos.
- Escolha as sub-redes: `us-east-2a`, `us-east-2b`, `us-east-2c`.

**Etapa 4: Configurar Tráfego e Escalabilidade**

- Defina os grupos de segurança do EC2. Por exemplo, crie um grupo `devops4` com:
  - Regras de entrada: Permita tráfego TCP de `0.0.0.0/0`.
  - Regras de saída: Permita todo o tráfego.
- Escolha uma instância compatível com a cota gratuita, como `t3.micro`.

**Etapa 5: Configurar Monitoramento e Logs**

- Mantenha as configurações padrão de monitoramento e relatórios de integridade.
- Escolha o Nginx como servidor de proxy.

**Etapa 6: Revisão Final**

- Revise todas as configurações, como nome da aplicação, perfis de serviço, sub-redes e IP público.
- Clique em **Enviar** para criar o ambiente.

## Ajustando a Imagem Docker e Subindo na AWS

### Expondo a Porta do Contêiner

Navegue até o diretório do projeto:

```bash
cd ./curso-react-alurabooks/
````

Abra o arquivo `Dockerfile`:

```bash
nano Dockerfile
```

Insira a linha abaixo entre o `RUN npm install` e o `COPY`:

```dockerfile
EXPOSE 3000
```

Salve e feche o arquivo (`Ctrl + X`, depois `Y` e `Enter`).

### Gerando um Novo Build da Imagem

```bash
docker build -t lcsrm/allbooks:1.2 .
```

* `-t`: Nomeia e versiona a imagem.
* `lcsrm/allbooks:1.2`: Nome da imagem e versão.
* O ponto (`.`) indica o diretório atual.

### Atualizando a Imagem no Docker Hub

```bash
docker push lcsrm/allbooks:1.2
```

O Docker enviará as camadas da imagem para o repositório.

## Criando o Arquivo Dockerrun.aws.json

O arquivo `Dockerrun.aws.json` informa ao Elastic Beanstalk qual imagem usar.

Crie o arquivo:

```bash
nano Dockerrun.aws.json
```

Conteúdo:

```json
{
  "AWSEBDockerrunVersion": "1",
  "Image": {
    "Name": "lcsrm/allbooks:1.2",
    "Update": "true"
  },
  "Ports": [
    {
      "ContainerPort": 3000
    }
  ]
}
```

Salve e feche o arquivo.

## Verificando no Docker Hub

* Acesse o [Docker Hub](https://hub.docker.com).
* Pesquise por `lcsrm/allbooks` e verifique a presença da tag `1.2`.

## Próximo Passo

Com o `Dockerrun.aws.json` criado e a imagem publicada, o ambiente está pronto para ser implantado no Elastic Beanstalk.

## Subindo a Aplicação na Nuvem

### Passo 1: Fazer Upload e Implantar

* Acesse o Elastic Beanstalk na AWS.
* Clique no botão **Fazer Upload e Implantar**.
* Escolha o arquivo `Dockerrun.aws.json`.
* Confirme o nome da versão (ex: `allbooks-version-1`).
* Clique em **Implantar**.

### Passo 2: Acompanhar o Processo de Implantação

* Acompanhe os logs e eventos para verificar o progresso.
* Se necessário, clique em **Ações** para:

  * Reconstruir o ambiente.
  * Encerrar o ambiente (para evitar custos).

### Passo 3: Testar a Aplicação

Após a implantação, será exibido um link de acesso como:

```
http://teste-env.eba-5whbwxbp.us-east-2.elasticbeanstalk.com
```

* Acesse o link no navegador.
* Verifique se a aplicação AllBooks está funcionando e exibindo os livros corretamente.

## Reflexão e Próximos Passos

A aplicação AllBooks agora está containerizada e funcionando na nuvem, acessível via web.

### Conceitos Avançados a Explorar

* **Docker Compose**:
  Facilita o gerenciamento de múltiplos containers (front-end, back-end, banco de dados), evitando comandos manuais.

* **Kubernetes**:
  Plataforma para orquestração de containers em larga escala, oferecendo alta disponibilidade e escalabilidade.


[🔝 Voltar ao topo](#sumário-interativo)

---














