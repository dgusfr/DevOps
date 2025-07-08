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
- [Docker e Comunicação em Rede](#docker-e-comunicação-em-rede)
  - [O Desafio da Comunicação](#o-desafio-da-comunicação)
  - [A Rede Padrão: `bridge`](#a-rede-padrão-bridge)
  - [Experimento Prático: Comunicação via IP](#experimento-prático-comunicação-via-ip)
    - [3.1. Preparando o Ambiente](#31-preparando-o-ambiente)
    - [3.2. Obtendo os Endereços IP](#32-obtendo-os-endereços-ip)
    - [3.3. Testando a Conexão com `ping`](#33-testando-a-conexão-com-ping)
  - [Limitações da Comunicação via IP](#limitações-da-comunicação-via-ip)
  - [A Solução: Redes Definidas pelo Usuário e DNS](#5-a-solução-redes-definidas-pelo-usuário-e-dns)
    - [5.1. Definindo o Nome de um Contêiner](#51-definindo-o-nome-de-um-contêiner)
    - [5.2. Criando uma Rede `bridge` Personalizada](#52-criando-uma-rede-bridge-personalizada)
    - [5.3. Associando Contêineres à Rede Personalizada](#53-associando-contêineres-à-rede-personalizada)
  - [Exemplo Prático: Comunicação via Nome](#6-exemplo-prático-comunicação-via-nome)
  - [Resolução Automática de DNS](#7-resolução-automática-de-dns)
- [Conectando uma Aplicação a um Banco de Dados](#conectando-uma-aplicação-a-um-banco-de-dados)
  - [Cenário e Preparação do Ambiente](#cenário-e-preparação-do-ambiente)
  - [Configurando a Rede e os Contêineres](#configurando-a-rede-e-os-contêineres)
    - [2.1. Criando a Rede Bridge](#21-criando-a-rede-bridge)
    - [2.2. Executando o Contêiner do Banco de Dados (MongoDB)](#22-executando-o-contêiner-do-banco-de-dados-mongodb)
    - [2.3. Executando o Contêiner da Aplicação (alura-books)](#23-executando-o-contêiner-da-aplicação-alura-books)
  - [Testando a Comunicação](#3-testando-a-comunicação)
  - [Simulação de Falha e Recuperação](#4-simulação-de-falha-e-recuperação)
  - [Limitações da Abordagem Manual e Próximos Passos](#5-limitações-da-abordagem-manual-e-próximos-passos)


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

## Solução com Contêineres

Os contêineres encapsulam o software e suas dependências, garantindo que ele funcione da mesma forma em qualquer ambiente. Eles fornecem:

- **Isolamento**: Cada contêiner funciona de forma independente, como se fosse uma aplicação em sua própria máquina.
- **Eficiência**: Não é necessário executar sistemas operacionais completos, reduzindo o consumo de recursos.
- **Reprodutibilidade**: Asseguram que a aplicação tenha o mesmo comportamento em desenvolvimento, teste e produção.

---



## Camadas Docker: Como Sua Imagem é Feita

O Docker é construído em **camadas**, que são criadas quando você executa o `docker build` da sua imagem. Pense nisso como um bolo em fatias.

Cada instrução no seu **Dockerfile** (como `FROM`, `RUN`, `COPY`) gera uma nova camada. Essas camadas são como "fatias" que, juntas, formam a imagem final do seu aplicativo.

A grande sacada é que, se você mudar apenas uma dessas camadas, o Docker é inteligente: ele **reaproveita as outras camadas** que não foram alteradas na hora de construir sua imagem novamente. Isso é o que chamamos de **cache**. 

O **cache** funciona como uma memória rápida, guardando as partes da imagem que já estão prontas. Assim, o processo de construção se torna muito mais rápido e você economiza espaço em disco, já que o Docker não precisa refazer tudo do zero.

---

## As Raízes Linux do Docker: cgroups e Namespaces

Esse conceito de camadas e o funcionamento do Docker vêm de tecnologias fundamentais do **Linux**, mais precisamente dos **cgroups** e **namespaces**. Vamos entender de forma simples o que eles são e como funcionam:

### Namespaces: As "Bolhas" de Isolamento

Isolam diferentes aspectos dos contêineres:

- **PID**: Isolamento dos processos.
- **NET**: Isolamento de redes e interfaces.
- **IPC**: Isolamento da comunicação entre processos.
- **MNT**: Isolamento do sistema de arquivos.
- **UTS**: Permite que o contêiner tenha seu próprio nome de host.

Essa capacidade de criar ambientes isolados é o que faz os **containers Docker** parecerem mini-computadores independentes.

---

### cgroups: O "Gerente" de Recursos

São como um "gerente de recursos" que controlam o uso de recursos (CPU, memória, I/O) de cada contêiner.

Com os cgroups, você pode dizer, por exemplo:

* "Este aplicativo só pode usar **50% da CPU**."
* "Ele só pode usar **1 GB de memória RAM**."

---

### A Relação com o Docker

O **Docker** usa os **namespaces** para criar o **isolamento** que faz cada container ser um ambiente independente. E ele usa os **cgroups** para **limitar** os recursos (CPU, memória, etc.) que cada um desses containers pode usar.

Em resumo, o Docker se apoia nessas tecnologias nativas do Linux para empacotar seus aplicativos de forma eficiente e segura, garantindo que eles funcionem de maneira consistente e sem interferir uns nos outros ou no sistema operacional.

## Diferenças Entre Máquinas Virtuais e Contêineres

Perfeito!
Com base nas suas orientações, reformulei o texto original mantendo seu tom didático, corrigindo pequenas repetições e acrescentando as melhorias que você pediu.
Incluí:
✅ Explicação melhorada sobre o kernel,
✅ Melhor analogia para containers,
✅ Reforço sobre isolamento,
✅ Pequena dica técnica sobre tipos de virtualização.

Segue a **versão reformulada**:

---

## 🧠 **Kernel: O Cérebro do Sistema Operacional**

O kernel é a parte central de um sistema operacional. Pense nele como o cérebro que gerencia todos os recursos do computador — CPU, memória, disco, rede — e permite que os softwares conversem com o hardware.
Ele faz tarefas essenciais como agendamento de processos, gerenciamento de memória e controle de acesso a dispositivos.

Uma dúvida comum é: *o kernel é sempre o mesmo?*
A resposta é não: cada sistema operacional tem seu próprio kernel.

* O Linux tem o kernel Linux.
* O Windows usa o NT kernel.
* O macOS utiliza um kernel baseado no Mach, dentro do projeto Darwin.

Todos eles desempenham funções parecidas, mas são implementados de formas diferentes, específicas para cada sistema.

---

## 🏢 **Virtualização: Dividindo o Hardware**

Virtualização é a técnica de dividir um único computador físico em partes menores, cada uma funcionando de forma independente.
É como pegar um prédio inteiro e transformá-lo em vários apartamentos: cada apartamento tem suas instalações, mas todos estão dentro do mesmo prédio.

---

### 🔍 **Pequena dica técnica**

Existem diferentes tipos de virtualização:

* **Virtualização plena (hardware)**: cria várias “máquinas completas”, cada uma com seu próprio sistema operacional. É o que acontece nas máquinas virtuais.
* **Virtualização no nível do sistema operacional** (ou containerização): cria múltiplos ambientes isolados que compartilham o mesmo kernel. É o que acontece com os containers.

Ambas servem para isolar aplicações e aproveitar melhor o hardware, mas funcionam em camadas diferentes.

---

## 🖥 **Máquinas Virtuais (VMs): Servidores Completos Dentro de Um Só**

Uma máquina virtual (VM) é como ter um apartamento inteiro no prédio, com sua cozinha, banheiro e estrutura completa.
Na prática, dentro de um servidor físico, podemos ter várias VMs, e cada uma roda seu próprio sistema operacional completo (Linux, Windows etc.), com todos os arquivos, bibliotecas e programas.

Para que isso seja possível, usamos um software chamado **Hypervisor**, que fica entre o hardware físico e as VMs.
Ele aloca recursos (CPU, memória, rede) para cada VM e garante que elas não interfiram entre si.

Existem dois tipos de hypervisors:

* **Tipo 1 (bare-metal)**: roda direto no hardware.
* **Tipo 2 (hosted)**: roda como aplicativo dentro de um sistema operacional já existente.

Por cada VM carregar um sistema operacional inteiro, elas são mais pesadas e consomem mais CPU, RAM e disco.
Mas oferecem isolamento muito forte: se uma VM falhar, normalmente não afeta as outras nem o host.
Por isso, são usadas quando é preciso rodar sistemas diferentes ou garantir isolamento máximo.

---

## 📦 **Containers: Aplicações Isoladas e Leves**

Containers também são uma forma de virtualização, mas funcionam de forma diferente.
Eles não criam uma máquina inteira com seu próprio sistema operacional. Em vez disso, todos os containers compartilham o mesmo kernel do sistema hospedeiro.

É como se, em vez de vários apartamentos completos, você tivesse várias “salas comerciais” dentro do mesmo andar: cada sala é independente para trabalhar, mas todas compartilham estrutura como água, energia e fundação do prédio.

No Linux, isso é feito usando:

* **Namespaces**: criam “bolhas” que isolam processos, rede e sistema de arquivos.
* **cgroups (control groups)**: limitam o uso de recursos como CPU e memória.

Por não carregarem um sistema operacional completo, containers são muito mais leves e rápidos de criar ou destruir, usando bem menos recursos.

---

## 🔒 **Isolamento: Diferenças Importantes**

* **VMs** oferecem isolamento completo: cada uma tem seu kernel e sistema próprio.
* **Containers** isolam o ambiente de execução (processos, rede, sistema de arquivos), mas compartilham o kernel do host.
  Isso traz eficiência e velocidade, mas o isolamento entre containers é mais limitado do que entre VMs.

---

✅ **Em resumo**:

* O kernel é o núcleo que conecta aplicativos ao hardware.
* Virtualização permite dividir um servidor em vários ambientes isolados.
* VMs virtualizam o hardware inteiro, com SO completo e forte isolamento.
* Containers virtualizam só o ambiente do usuário, são mais leves e rápidos, mas compartilham o kernel.

---



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

-----

### Criando e Usando Volumes

**1. Listando Volumes Existentes**

Para ver os volumes que você já tem no Docker, use este comando:

```bash
docker volume ls
```

Se você nunca criou um volume antes, a lista estará vazia.

-----

**2. Criando um Volume**

Vamos criar um novo volume chamado `novo-volume`:

```bash
docker volume create novo-volume
```

Para confirmar que ele foi criado, liste os volumes novamente:

```bash
docker volume ls
```

-----

### Onde os arquivos do volume são armazenados?

Os volumes Docker são gerenciados pelo próprio Docker e, por padrão, ficam em um local específico no seu sistema de arquivos.

Primeiro, saia de qualquer contêiner que você esteja usando (se aplicável) com `exit`.

Em seguida, entre como superusuário, pois os diretórios do Docker geralmente exigem permissões de root:

```bash
sudo su
```

(Insira sua senha se for solicitado.)

O diretório onde o Docker armazena suas informações, incluindo volumes, é `/var/lib/docker`. Vamos até ele:

```bash
cd /var/lib/docker/
```

Se você listar o conteúdo dessa pasta (`ls`), verá vários diretórios como `plugins`, `buildkit`, `image`, `overlay`, e, o que nos interessa, `volumes`. Acesse-o:

```bash
cd volumes
```

Dentro de `volumes`, você pode listar o conteúdo (`ls`) e notará o seu `novo-volume` (ou `meu-volume` se você usou esse nome no seu exemplo anterior) lá dentro\! Acesse-o:

```bash
cd novo-volume
```

Agora, dê um `ls` novamente. Você provavelmente verá uma pasta chamada `_data`. Essa é a pasta onde os dados reais do seu volume são armazenados. Acesse-a:

```bash
cd _data
```

Qualquer arquivo que você colocar neste volume dentro de um contêiner aparecerá aqui. Por exemplo, se você tivesse um `um-arquivo-qualquer` dentro do seu volume, ele estaria aqui.

Então, o caminho completo para os seus arquivos de volume é: `/var/lib/docker/volumes/novo-volume/_data`. É importante notar que este local é **totalmente gerenciado pelo Docker**.

-----

### Gerenciamento de Volumes

O Docker oferece uma interface de linha de comando robusta para gerenciar seus volumes. Se você sair do modo superusuário (com `exit`) e digitar `docker volume` sem nenhum outro argumento, você verá uma lista dos comandos disponíveis para gerenciamento de volumes:

  * `create`: Para criar novos volumes.
  * `inspect`: Para visualizar detalhes sobre um volume específico.
  * `ls`: Para listar todos os volumes.
  * `prune`: Para remover volumes que não estão sendo usados por nenhum contêiner.
  * `rm`: Para remover um volume específico, mesmo que esteja em uso (use com cautela).

Isso significa que você pode gerenciar seus volumes de forma eficiente através dos comandos do Docker, sem precisar navegar diretamente pelo sistema de arquivos do seu sistema operacional. 

O Docker abstrai e gerencia essa parte para você, garantindo que os volumes estejam sempre nesse diretório dedicado, independentemente de detalhes específicos da sua estrutura de pastas.

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
---


# Docker e Comunicação em Rede

## O Desafio da Comunicação

Aplicações modernas são distribuídas. Um front-end precisa consumir uma API, que por sua vez consulta um banco de dados. Por padrão, contêineres Docker são ambientes isolados. Como garantir que eles se comuniquem de forma segura e eficiente? A resposta está nas redes Docker.

## A Rede Padrão: `bridge`

Quando o Docker é instalado, ele cria automaticamente uma rede virtual do tipo `bridge`. Todo contêiner, por padrão, é conectado a essa rede. Pense nela como uma rede local (LAN) privada para seus contêineres, permitindo que eles se comuniquem entre si, ao mesmo tempo que os mantém isolados da rede do host.

Para listar as redes Docker disponíveis em sua máquina, use:

```bash
docker network ls
```

Você verá as redes padrão: `bridge`, `host` e `none`. Focaremos na `bridge`.

-----

## Experimento Prático: Comunicação via IP

Vamos provar que dois contêineres na mesma rede `bridge` podem se comunicar usando seus endereços IP.

### 3.1. Preparando o Ambiente

1.  **Inicie o primeiro contêiner (C1):**

    ```bash
    docker run -it -d --name c1 ubuntu bash
    ```

      * `-d`: Executa o contêiner em modo "detached" (em segundo plano).
      * `--name c1`: Nomeia o contêiner como `c1` para facilitar a referência.

2.  **Inicie o segundo contêiner (C2):**

    ```bash
    docker run -it -d --name c2 ubuntu bash
    ```

### 3.2. Obtendo os Endereços IP

Cada contêiner na rede `bridge` recebe um endereço IP único. Use o comando `docker inspect` para descobri-los.

1.  **Obtenha o IP do C1:**

    ```bash
    docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' c1
    ```

2.  **Obtenha o IP do C2:**

    ```bash
    docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' c2
    ```

Anote os endereços IP retornados.

### 3.3. Testando a Conexão com `ping`

Agora, vamos acessar o primeiro contêiner (`c1`) e tentar "pingar" o segundo (`c2`) usando seu endereço IP.

1.  **Acesse o terminal do `c1`:**

    ```bash
    docker exec -it c1 bash
    ```

2.  Dentro do contêiner, **instale as ferramentas de rede** (o `ping` não vem por padrão na imagem mínima do Ubuntu):

    ```bash
    apt-get update && apt-get install -y iputils-ping
    ```

3.  **Execute o `ping`** para o endereço IP do `c2` (substitua `<IP_DO_CONTAINER_2>` pelo IP que você anotou):

    ```bash
    ping <IP_DO_CONTAINER_2>
    ```

Você verá pacotes sendo enviados e recebidos, confirmando a comunicação. Para sair do `ping`, pressione `Ctrl+C`. Para sair do contêiner, digite `exit`.

-----

### 4. Limitações da Comunicação via IP

Embora funcional, usar o endereço IP diretamente é uma má prática em ambientes dinâmicos por duas razões principais:

  * **Instabilidade:** O endereço IP de um contêiner **não é garantido**. Se você reiniciar ou recriar um contêiner, ele provavelmente receberá um novo IP, quebrando a comunicação.
  * **Manutenção Difícil:** Configurar aplicações para apontar para IPs fixos torna o sistema frágil e difícil de manter. Qualquer mudança na infraestrutura exige reconfiguração manual.

**Conclusão:** A comunicação via IP na rede `bridge` padrão serve para demonstrar a conectividade, mas não é uma solução para produção. 

Entendido. Dando continuidade ao capítulo anterior, aqui está o restante do conteúdo, com a numeração e formatação ajustadas para o estilo direto da apostila.

-----

### 5. A Solução: Redes Definidas pelo Usuário e DNS

Vimos que usar IPs para a comunicação entre contêineres é frágil. A solução robusta do Docker é usar **redes personalizadas**, que habilitam um sistema de DNS interno. Isso permite que os contêineres se encontrem usando seus **nomes**, um identificador estável e previsível.

#### **5.1. Definindo o Nome de um Contêiner**

O primeiro passo é dar um nome fixo a um contêiner no momento de sua criação usando a flag `--name`.

**Exemplo:**

```bash
docker run -it --name ubuntu1 ubuntu bash
```

Neste caso, o contêiner recebe o nome `ubuntu1`, que servirá como seu "hostname" dentro de uma rede personalizada.

#### **5.2. Criando uma Rede `bridge` Personalizada**

A rede `bridge` padrão do Docker não possui o DNS interno habilitado. Para isso, precisamos criar nossa própria rede.

**Comando:**

```bash
docker network create --driver bridge minha-bridge
```

Este comando cria uma nova rede chamada `minha-bridge`. Contêineres conectados a esta rede poderão se comunicar pelo nome.

#### **5.3. Associando Contêineres à Rede Personalizada**

Ao iniciar um contêiner, utilize a flag `--network` para conectá-lo à rede que você criou.

**Exemplo:**

```bash
docker run -it -d --name ubuntu1 --network minha-bridge ubuntu bash
```

  * O contêiner `ubuntu1` é criado e imediatamente associado à rede `minha-bridge`.
  * `-d`: executa o contêiner em segundo plano.

Para verificar se o contêiner está na rede correta, use o comando `inspect`:

```bash
docker inspect ubuntu1
```

Na seção "Networks" da saída JSON, você confirmará que ele está conectado à `minha-bridge`.

-----

### 6. Exemplo Prático: Comunicação via Nome

Vamos testar o sistema de resolução de nomes na prática. Criaremos dois contêineres na mesma rede personalizada e faremos um "pingar" o outro pelo nome.

1.  **Crie a rede (caso ainda não tenha criado):**

    ```bash
    docker network create minha-bridge
    ```

2.  **Crie o primeiro contêiner ("pingador"):**

    ```bash
    docker run -it -d --name ubuntu1 --network minha-bridge ubuntu bash
    ```

3.  **Crie o segundo contêiner ("alvo"):**

    ```bash
    docker run -d --name pong --network minha-bridge ubuntu sleep 1d
    ```

      * `sleep 1d`: É um comando simples para manter o contêiner `pong` em execução por 1 dia sem consumir recursos.

4.  **Acesse o contêiner `ubuntu1` e instale o `ping`:**

    ```bash
    docker exec -it ubuntu1 bash
    ```

    Dentro do contêiner, execute:

    ```bash
    apt-get update && apt-get install -y iputils-ping
    ```

5.  **Teste a comunicação usando o nome do contêiner:**
    Ainda dentro do `ubuntu1`, execute:

    ```bash
    ping pong
    ```

**Resultado:** O ping funcionará. O Docker resolveu o nome `pong` para o endereço IP interno correto do contêiner alvo, de forma automática.

-----

### 7. Resolução Automática de DNS

Este recurso é o pilar da comunicação entre contêineres em arquiteturas de microsserviços.

  * **Redes Definidas pelo Usuário (User-Defined Networks):** Qualquer rede `bridge` que você cria (`docker network create`) habilita automaticamente a resolução de DNS entre os contêineres conectados a ela.
  * **Como funciona:** O Docker mantém um mapeamento interno do nome de cada contêiner para seu respectivo endereço IP naquela rede. Quando um contêiner tenta se conectar a outro pelo nome, o Docker intercepta a requisição e a direciona para o IP correto.


---

Com certeza. Aqui está o conteúdo organizado como um novo capítulo da apostila, mantendo o estilo direto e a formatação correta.

-----

### Redes Especiais: `none` e `host`

Além das redes `bridge`, o Docker oferece modos de rede especiais para casos de uso específicos. Nesta seção, exploraremos os modos `none` (sem rede) e `host` (rede compartilhada com o hospedeiro).

-----

#### **1. A Rede `none`**

  * **Driver:** `null`
  * **Funcionamento:** Isola completamente o contêiner da rede. Ao ser associado à rede `none`, o contêiner não recebe uma interface de rede, não ganha um endereço IP e não pode se comunicar com o mundo externo, nem com outros contêineres.

##### **Exemplo Prático:**

1.  **Execute um contêiner na rede `none`:**

    ```bash
    docker run -d --name sem-rede --network none ubuntu sleep 1d
    ```

    Este comando cria um contêiner chamado `sem-rede`, que ficará em execução por um dia, totalmente desconectado da rede.

2.  **Verificação:**
    Inspecione o contêiner para confirmar a ausência de configurações de rede.

    ```bash
    docker inspect sem-rede
    ```

    Na seção `NetworkSettings`, você verá que ele está associado à rede `none` e não possui um endereço IP.

##### **Impacto e Casos de Uso:**

O contêiner fica em uma "bolha", completamente isolado. É útil para tarefas que não exigem rede, como processamento de dados em lote (onde os dados já estão dentro do contêiner) ou para testes de segurança que exigem um ambiente hermeticamente fechado.

-----

#### **2. A Rede `host`**

  * **Driver:** `host`
  * **Funcionamento:** Remove o isolamento de rede entre o contêiner e a máquina hospedeira (host). O contêiner passa a compartilhar a interface de rede do host diretamente. Na prática, qualquer porta que a aplicação dentro do contêiner abrir estará aberta diretamente no host.

##### **Exemplo Prático:**

1.  **Execute um contêiner na rede `host`:**
    Certifique-se de que a porta `3000` não esteja em uso em sua máquina.

    ```bash
    docker run -d --name app-host --network host aluradocker/app-node:1.0
    ```

      * **Atenção:** Note que **não** usamos a flag `-p`. O mapeamento de portas é desnecessário, pois o contêiner usa a rede do host diretamente.

2.  **Acesso à Aplicação:**
    A aplicação `app-node` (versão 1.0) roda na porta `3000`. Como o contêiner está na rede do host, você pode acessá-la diretamente no seu navegador:
    `http://localhost:3000`

##### **Observações Importantes:**

  * **Conflito de Portas:** O principal risco é o conflito de portas. Se outra aplicação (em contêiner ou não) já estiver usando a porta `3000` no host, o contêiner falhará ao iniciar.
  * **Segurança:** Este modo de rede reduz o isolamento, que é uma das principais vantagens dos contêineres. O contêiner tem acesso direto à interface de rede do host, o que pode representar um risco de segurança se a aplicação no contêiner for comprometida. É recomendado para cenários onde o desempenho da rede é extremamente crítico e o risco de segurança é controlado.

-----

#### **3. Resumo**

| Rede | Funcionamento | Caso de Uso Típico |
| :--- | :--- | :--- |
| **`none`** | Isola completamente o contêiner da rede. | Tarefas em lote, processamento de dados offline. |
| **`host`** | Remove o isolamento, compartilhando a rede do host. | Aplicações que exigem altíssimo desempenho de rede, onde a latência de uma rede `bridge` é um fator limitante. |

[🔝 Voltar ao topo](#sumário-interativo)

---

<br>
<br>
<br>

---

# Conectando uma Aplicação a um Banco de Dados

Neste capítulo, vamos aplicar os conceitos de rede para simular um cenário real: uma aplicação back-end que se comunica com um banco de dados, ambos rodando em contêineres Docker separados.

-----

## Cenário e Preparação do Ambiente

Nosso objetivo é conectar uma aplicação Node.js (`alura-books`) a um banco de dados (`MongoDB`). A comunicação ocorrerá através de uma rede `bridge` personalizada, usando o nome do contêiner do banco como hostname.

**Pré-requisitos:**
Primeiro, baixe as imagens Docker necessárias:

```bash
docker pull mongo:4.4.6
docker pull aluradocker/alura-books:1.0
```

*É fundamental usar a versão `4.4.6` do Mongo para garantir a compatibilidade com a aplicação.*

-----

## Configurando a Rede e os Contêineres

### **2.1. Criando a Rede Bridge**

Para que os contêineres possam se comunicar pelo nome, criaremos uma rede personalizada.

```bash
docker network create --driver bridge minha-bridge
```

Você pode verificar se a rede foi criada com `docker network ls`.

### **2.2. Executando o Contêiner do Banco de Dados (MongoDB)**

Vamos iniciar o contêiner do MongoDB, conectando-o à nossa rede e dando-lhe um nome específico que a aplicação espera.

```bash
docker run -d --network minha-bridge --name meu-mongo mongo:4.4.6
```

  * `-d`: Executa em modo detached (segundo plano).
  * `--network minha-bridge`: Conecta o contêiner à nossa rede.
  * `--name meu-mongo`: **Ponto crítico.** A aplicação `alura-books` está codificada para procurar o banco de dados no hostname `meu-mongo`.

### **2.3. Executando o Contêiner da Aplicação (alura-books)**

Agora, vamos executar a aplicação, conectando-a na mesma rede e expondo sua porta para que possamos acessá-la do nosso navegador.

```bash
docker run -d --network minha-bridge --name alurabooks -p 3000:3000 aluradocker/alura-books:1.0
```

  * `--network minha-bridge`: Conecta a aplicação à mesma rede do banco de dados.
  * `-p 3000:3000`: Mapeia a porta 3000 do contêiner para a porta 3000 da nossa máquina (host), permitindo o acesso externo.

-----

### 3. Testando a Comunicação

Com ambos os contêineres em execução, a aplicação `alurabooks` consegue encontrar e se comunicar com o `meu-mongo` através da rede `minha-bridge`.

1.  **Acesse a aplicação:** Abra seu navegador e vá para `http://localhost:3000`. A página deve carregar, mas sem dados.
2.  **Povoando o banco de dados:** Para inserir dados iniciais, acesse o endpoint `/seed` no navegador:
    `http://localhost:3000/seed`
3.  **Verifique os dados:** Volte para `http://localhost:3000` e atualize a página. Os livros agora devem aparecer, confirmando que a aplicação se comunicou com o banco, gravou e leu os dados.

![alt text](images/test_comun.png)

-----

### 4. Simulação de Falha e Recuperação

Para provar a dependência entre os serviços, vamos parar o contêiner do banco de dados.

1.  **Pare o MongoDB:**

    ```bash
    docker stop meu-mongo
    ```

2.  **Teste a aplicação:** Atualize a página no navegador. A aplicação irá falhar ou exibir uma tela de erro, pois não consegue mais se conectar ao banco.

3.  **Restaure o serviço:**

    ```bash
    docker start meu-mongo
    ```

4.  **Verifique novamente:** Atualize o navegador. A conexão é restaurada e os dados voltam a ser exibidos (pois os dados no volume do Mongo foram preservados).

-----

### 5. Limitações da Abordagem Manual e Próximos Passos

Conseguimos criar um ambiente funcional, mas o processo foi inteiramente manual. Isso levanta questões críticas para um ambiente de produção:

  * A inicialização manual é propensa a erros e não é escalável.
  * Como garantir que o banco de dados inicie sempre *antes* da aplicação?
  * Como gerenciar dezenas ou centenas de serviços interconectados?

A resposta para essas perguntas está na **orquestração de contêineres**. 

A abordagem manual não é viável para produção. Ferramentas como o **Docker Compose** foram criadas para resolver exatamente esses problemas, permitindo definir e gerenciar aplicações multi-contêiner de forma declarativa e automatizada. Este será o foco dos nossos próximos capítulos.

[🔝 Voltar ao topo](#sumário-interativo)

---

<br>
<br>
<br>

---












