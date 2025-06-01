# Comandos Linux

## Sumário

- [1. Criação e Gestão de Arquivos e Diretórios](#1-criação-e-gestão-de-arquivos-e-diretórios)
- [2. Navegação entre Diretórios](#2-navegação-entre-diretórios)
- [3. Movendo Arquivos e Diretórios](#3-movendo-arquivos-e-diretórios)
- [4. Copiando e Renomeando Arquivos](#4-copiando-e-renomeando-arquivos)
- [5. Controle e Acesso ao Sistema](#5-controle-e-acesso-ao-sistema)
- [6. Redirecionando Saídas](#6-redirecionando-saídas)
- [7. Analisando Processos em Execução](#7-analisando-processos-em-execução)
- [8. Trabalhando com Textos](#8-trabalhando-com-textos)
- [9. Manipulação de Compactação](#9-manipulação-de-compactação)
- [10. Comandos Úteis Adicionais](#10-comandos-úteis-adicionais)
- [11. Gerenciamento de Usuários e Permissões](#11-gerenciamento-de-usuários-e-permissões)
- [12. Protocolos de Rede e Memória](#12-protocolos-de-rede-e-memória)
- [13. Gerenciamento de Pacotes e Serviços](#13-gerenciamento-de-pacotes-e-serviços)
- [14. GNU Nano](#14-gnu-nano)
- [15. Monitoramento](#15-monitoramento)
- [16. Scripts de Automação](#16-scripts-de-automação)

---

### 1. Criação e Gestão de Arquivos e Diretórios



* **mkdir**: Cria um novo diretório.

```bash
$ mkdir nova_pasta
```

* **rmdir**: Remove diretórios vazios.

```bash
$ rmdir pasta_vazia
```

* **rm**: Remove arquivos ou diretórios (use com cuidado).

```bash
$ rm arquivo.txt                 # Remove um arquivo
$ rm -r pasta_com_conteudo       # Remove um diretório com conteúdo
```

* **touch**: Cria arquivos vazios.

```bash
$ touch arquivo1.txt arquivo2.txt
```

* **cat**: Exibe ou cria arquivos.

```bash
$ cat arquivo.txt                # Exibe o conteúdo de um arquivo
$ cat > novo_arquivo.txt         # Cria um arquivo e escreve conteúdo
```

* **chmod**: Altera permissões de arquivos.

```bash
$ chmod 755 script.sh            # Permissão para leitura, gravação e execução pelo dono, leitura/execução para grupo e outros
```

Tabela de permissões do `chmod`

| Permissão | Número | Significado para o Dono/Grupo/Outros |
| --------- | ------ | ------------------------------------ |
| `---`     | 0      | Nenhuma permissão                    |
| `--x`     | 1      | Execução somente                     |
| `-w-`     | 2      | Escrita somente                      |
| `-wx`     | 3      | Escrita e execução                   |
| `r--`     | 4      | Leitura somente                      |
| `r-x`     | 5      | Leitura e execução                   |
| `rw-`     | 6      | Leitura e escrita                    |
| `rwx`     | 7      | Leitura, escrita e execução          |

Esses números são combinados em três grupos (dono, grupo e outros) para formar o número final que você usa com o `chmod`. Por exemplo:

* `chmod 755 arquivo`:

  * Dono: 7 (rwx)
  * Grupo: 5 (r-x)
  * Outros: 5 (r-x)

* `chmod 644 arquivo`:

  * Dono: 6 (rw-)
  * Grupo: 4 (r--)
  * Outros: 4 (r--)


* **chown**: Altera o dono e grupo de um arquivo.

```bash
$ chown usuario:grupo arquivo.txt
```

---

### 2. Navegação entre Diretórios

- **pwd**: Exibe o diretório atual.

```bash
$ pwd
/home/usuario
````

* **ls**: Lista arquivos e pastas do diretório atual.

```bash
$ ls
documento.txt  imagens/  projeto/
```

* **cd**: Change Directory, Acessa um diretório.

```bash
$ cd Downloads  
$ cd ..         # Retorna ao diretório anterior
```

* **find**: Busca arquivos e diretórios.

```bash
$ find /caminho -name "arquivo.txt"  # Busca por nome
$ find /caminho -type f -size +1M     # Busca arquivos maiores que 1MB
$ find /caminho -type f -atime +7     # Arquivos não acessados nos últimos 7 dias
$ find /caminho -type f -iname "ARQUIVO.txt"  # Busca insensível a maiúsculas
```

---

### 3. Movendo Arquivos e Diretórios

Para organizar melhor os arquivos, podemos criar subdiretórios e mover arquivos entre eles. Por exemplo:

* Criação de um subdiretório:

```bash
mkdir ideias
```

* Listagem do conteúdo:

```bash
ls
```

Retorno esperado: `ideias  projeto_ideas.txt`

* Movendo o arquivo `projeto_ideas.txt` para o subdiretório `ideias`:

```bash
mv projeto_ideas.txt ideias/
```

* Verificação da movimentação:

```bash
ls
ls ideias/
```

* Criação de um novo diretório chamado `rascunho`:

```bash
mkdir rascunho
```

* Movendo o diretório `rascunho` para dentro de `ideias`:

```bash
mv rascunho ideias/
```

* Verificação final:

```bash
ls ideias/
```

As permissões são exibidas com:

```bash
ls -al
```

No formato:

```
drwxr-xr-x
```

Sendo:

* `d`: diretório
* `rwx`: proprietário (leitura, escrita, execução)
* `r-x`: grupo e outros (leitura, execução)

Isso facilita a gestão e organização dos arquivos e diretórios no Linux.

---

### 4. Copiando e Renomeando Arquivos


* Para duplicar um arquivo:

```bash
cp arquivo_original arquivo_copia
```

Exemplo:

```bash
cp projeto_ideias.txt projeto_ideias_v1.txt
```

Isso cria uma cópia chamada `projeto_ideias_v1.txt`.

#### Renomeando Arquivos e Diretórios

* Para renomear:

```bash
mv nome_antigo nome_novo
```

Exemplo:

```bash
mv rascunho modelo
```

Isso renomeia o diretório `rascunho` para `modelo`.


#### Movendo Arquivos entre Diretórios

* Para mover arquivos para outro diretório:

```bash
mv arquivo_a_mover diretorio_destino
```

Exemplo:

```bash
mv projeto_ideias_v1.txt modelo/
```

Isso move o arquivo para dentro do diretório `modelo`.


---

### 5. Controle e Acesso ao Sistema



* **exit**: Termina a sessão no terminal.

```bash
$ exit
```

* **logout**: Desloga do usuário atual no sistema.

```bash
$ logout
```

* **passwd**: Altera a senha do usuário.

```bash
$ passwd
```

* **ssh**: Conecta a um servidor remoto.

```bash
$ ssh usuario@servidor.com
```

#### sudo ("super user do")

* **Descrição**: O `sudo` é usado para executar comandos como superusuário, permitindo realizar ações que exigem privilégios administrativos.

> **Importante:** Use o `sudo` com cuidado, pois ele permite modificar arquivos de configuração e afetar a integridade do sistema.

* **Listar conteúdo do diretório root**:
  Para listar o conteúdo do diretório root com privilégios elevados:

```bash
sudo ls /root
```

O sistema solicitará a senha configurada durante a instalação do Ubuntu. Dentro do diretório root, encontraremos o diretório `snap`, que é uma biblioteca.

* **Comandos internos do shell**:
  O `sudo` funciona apenas para comandos externos do shell. Comandos internos, como o `cd`, que alteram o estado do shell, não podem ser usados diretamente com o `sudo`.

**Exemplo:**

```bash
sudo cd ./root
```

Isso retorna um erro porque `cd` é um comando interno do shell.

* **Iniciar uma sessão como superusuário**:
  Para acessar o diretório `/root`, precisamos iniciar uma sessão como superusuário usando:

```bash
sudo -i
```

Esse comando inicia uma sessão com privilégios administrativos com o usuário root, permitindo acessar e modificar qualquer parte do sistema.

> **Aviso:** Tenha cuidado ao usar essa sessão, pois você tem permissão para realizar qualquer ação no sistema.

* **Acessar o arquivo de configuração sudoers**:
  O arquivo `sudoers` configura e define as permissões para que os usuários possam executar ações administrativas no sistema. Para acessar o conteúdo desse arquivo:

```bash
cat /etc/sudoers
```

O arquivo lista os usuários com permissão para usar o `sudo`.

* **Saindo do modo superusuário**:
  Após realizar alterações ou acessar diretórios restritos como superusuário, é importante sair desse modo:

```bash
exit
```

Isso retorna à navegação como usuário normal, sem permissões administrativas.

> **Aviso:** Use o modo de superusuário apenas quando necessário para evitar erros que possam impactar o sistema.


---

### 6. Redirecionando Saídas


```bash
pgrep nginx > /dev/null
```

Redireciona a saída do comando para o dispositivo `/dev/null` (uma "lixeira" do sistema), evitando exibir qualquer informação no terminal.

**Comando com Redireção de Erros:**

```bash
pgrep nginx &> /dev/null
```

Redireciona tanto as saídas padrão quanto os erros para o descarte.


---

### 7. Analisando Processos em Execução

#### Comando `top`

O comando `top` (table of processes) é utilizado para visualizar os processos em execução em tempo real. Para utilizá-lo, basta abrir o terminal e digitar:

```bash
top
```

Ao executar o comando, você verá uma tabela dinâmica com informações sobre os processos em execução.
Na parte superior da tabela, você encontrará várias siglas e abreviações. Vamos entender cada uma delas:

* **%CPU**: Percentual de utilização da CPU pelo processo.
* **%MEM**: Percentual de utilização da memória pelo processo.
* **PID**: Número de identificação do processo (process id).
* **USER**: Usuário que está utilizando o processo.
* **PR**: Prioridade geral do processo.
* **NI**: Nice value (valor agradável) do processo, que influencia na prioridade.
* **VIRT**: Memória virtual usada pelo processo.
* **RES**: Memória residente usada (realmente alocada como RAM).
* **SHR**: Memória compartilhada usada pelo processo.
* **S**: Estado do processo (S = sleeping, R = running, T = stopped).


#### Exemplo de Ordenação

* Execute o comando `top`.
* Pressione `P` para ver os processos ordenados pelo uso da CPU.
* Pressione `M` para ver os processos ordenados pelo uso da memória.


#### Comando `ps`

O comando `ps` (**Process Status**) é a base para a análise de processos. Ao executá-lo sem opções, você obtém uma visão limitada dos processos ativos no momento:

```bash
ps
```


#### Comando `ps aux`

Para obter uma visão mais ampla dos processos, utilizamos o comando `ps aux`:

```bash
ps aux
```

Esse comando apresenta uma tabela com informações detalhadas sobre todos os processos em execução. Os principais campos são:

* **USER**: Usuário do processo
* **PID**: Número de identificação do processo
* **%CPU**: Percentual de CPU utilizado
* **%MEM**: Percentual de memória utilizado
* **VSZ**: Memória virtual utilizada
* **RSS**: Memória RAM alocada
* **TTY**: Tipo de terminal utilizado
* **STAT**: Status do processo
* **START**: Momento em que o processo foi iniciado
* **TIME**: Tempo de execução
* **COMMAND**: Comando vinculado ao processo

---

### 8. Trabalhando com Textos

* **cat**: Exibe o conteúdo de um arquivo.

```bash
$ cat arquivo.txt
```

* **cut**: Extrai campos de um arquivo.

```bash
$ cut -d "," -f 1 arquivo.csv  # Extrai a primeira coluna de um CSV
```

* **head** e **tail**: Exibem linhas do início ou fim de um arquivo.

```bash
$ head -n 5 arquivo.txt   # Exibe as 5 primeiras linhas
$ tail -n 10 arquivo.txt  # Exibe as 10 últimas linhas
```

* **more** e **less**: Exibem o conteúdo de arquivos de forma paginada.

```bash
$ more arquivo.txt
$ less arquivo.txt
```

* **grep**: Pesquisa por padrões em arquivos.

```bash
$ grep "padrão" arquivo.txt        # Encontra linhas que contêm "padrão"
$ grep -r "padrão" diretorio/     # Pesquisa recursivamente em um diretório
```

---

### 9. Manipulação de Compactação

* **tar**: Cria ou extrai arquivos compactados.

```bash
$ tar -czvf arquivo.tar.gz pasta/    # Compacta com gzip
$ tar -xzvf arquivo.tar.gz           # Extrai arquivo compactado
```

* **gzip/bzip2**: Compacta arquivos individualmente.

```bash
$ gzip arquivo.txt       # Compacta o arquivo para arquivo.txt.gz
$ bzip2 arquivo.txt      # Compacta o arquivo para arquivo.txt.bz2
```

* **zip/unzip**: Compacta e descompacta arquivos ZIP.

```bash
$ zip arquivos.zip arquivo1 arquivo2
$ unzip arquivos.zip
```

---

### 10. Comandos Úteis Adicionais

* **clear**: Limpa o terminal.

```bash
$ clear
```

* **df -h**: Exibe o uso do sistema de arquivos.

```bash
$ df -h
```

* **free -h**: Exibe informações sobre o uso de memória.

```bash
$ free -h
```

* **sudo apt-get update && sudo apt-get upgrade**: Atualiza o sistema.

```bash
$ sudo apt-get update && sudo apt-get upgrade
```

* **updatedb/locate**: Atualiza o banco de dados e busca arquivos.

```bash
$ sudo updatedb
$ locate arquivo.txt
```

* **which**: Localiza o caminho de um executável.

```bash
$ which ls
```

---

### 12. Gerenciamento de Usuários e Permissões

* **adduser**: Adiciona um novo usuário.

```bash
$ sudo adduser novo_usuario
```

* **usermod**: Modifica informações de um usuário.

```bash
$ sudo usermod -aG grupo usuario   # Adiciona o usuário a um grupo
```

* **chown**: Altera o dono e grupo de um arquivo.

```bash
$ sudo chown usuario:grupo arquivo.txt
```

* **chmod**: Modifica permissões de arquivos.

```bash
$ chmod 755 arquivo.txt
```

---

### 13. Protocolos de Rede e Memória



* **ping**: Testa a conectividade com um host.

```bash
$ ping google.com
```

* **ip addr**: Exibe informações de rede.

```bash
$ ip addr
```

* **ip route**: Exibe a tabela de roteamento.

```bash
$ ip route
```


---

### 14. Gerenciamento de Pacotes e Serviços

* **apt-get**: Instala e gerencia pacotes no sistema.

```bash
$ sudo apt-get install pacote   # Instala um pacote
$ sudo apt-get remove pacote    # Remove um pacote
```

* **service/systemctl**: Gerencia serviços do sistema.

```bash
$ sudo service apache2 start    # Inicia um serviço
$ sudo systemctl restart apache2
```

---

### 15. GNU Nano



O **GNU Nano** é um editor de texto simples e poderoso que vem pré-instalado em muitas distribuições Linux. Ele é usado diretamente no terminal e foi projetado para ser fácil de usar, mesmo para iniciantes. Nano é particularmente útil para editar arquivos de configuração ou criar scripts rapidamente, sem a necessidade de interfaces gráficas.

## Principais Características do GNU Nano

- **Simplicidade**: Ele é amigável, com comandos básicos exibidos na parte inferior da tela.
- **Baseado em Terminal**: Você pode usá-lo em servidores remotos ou sistemas sem interface gráfica.
- **Compatível com Vários Arquivos**: Permite abrir e editar múltiplos arquivos ao mesmo tempo.
- **Pesquisa e Substituição**: Inclui ferramentas de busca e substituição simples.
- **Open Source**: Faz parte do projeto GNU, ou seja, é gratuito e de código aberto.

---

## Como Usar o GNU Nano

No terminal, digite o comando abaixo seguido do nome do arquivo que você deseja editar:

```bash
nano
````

Exemplo para editar um arquivo chamado `config.txt`:

```bash
nano config.txt
```

Quando você abre o Nano, verá:

![Imagem GNU Nano acima](images/nano-interface.png)
Fonte: *[Autor](Autor)*

* **Conteúdo do arquivo**: A parte superior e central da tela mostra o texto do arquivo.
* **Linha de Status**: Abaixo do conteúdo, há informações como o nome do arquivo e o número da linha.
* **Atalhos de Comando**: Na parte inferior, comandos básicos aparecem com um símbolo de `^`. O `^` representa a tecla Ctrl.

#### Comandos Básicos no GNU Nano

| Atalho   | Descrição                             |
| -------- | ------------------------------------- |
| Ctrl + O | Salvar o arquivo.                     |
| Ctrl + X | Sair do editor.                       |
| Ctrl + W | Buscar um texto no arquivo.           |
| Ctrl + K | Cortar uma linha.                     |
| Ctrl + U | Colar o texto cortado anteriormente.  |
| Ctrl + G | Abrir o menu de ajuda.                |
| Ctrl + T | Verificar ortografia (se habilitado). |

#### Editando Arquivos

* **Abrir e Digitar**: Após abrir o arquivo, basta começar a digitar ou navegar usando as setas do teclado.
* **Salvar as Alterações**:
  Pressione `Ctrl + O` para salvar.
  O Nano perguntará se você deseja salvar no mesmo arquivo ou em outro nome.
  Pressione `Enter` para confirmar.

#### Sair do Nano

Para sair, pressione `Ctrl + X`.
Se houver alterações não salvas, o Nano perguntará:

```
Save modified buffer? (Answering "No" will DISCARD changes)
```

* Digite `Y` para salvar.
* Digite `N` para sair sem salvar.


---

### 15. Monitoramento

Monitorando Processos com o Comando top

Exibe uma visão dinâmica e em tempo real dos processos em execução no sistema.  
Mostra o consumo de recursos (CPU, memória) e o estado dos processos.

### Exemplo de Saída:

| PID  | USER   | PR | NI | VIRT | RES  | SHR  | S | %CPU | %MEM | TIME+   | COMMAND  |
|------|--------|----|----|------|------|------|---|------|------|----------|----------|
| 1    | root   | 20 | 0  | 1015 | 1286 | 8456 | S | 0.0  | 0.3  | 0:01.29  | systemd  |
| 2    | root   | 20 | 0  | 0    | 0    | 0    | S | 0.0  | 0.0  | 0:00.01  | kthreadd |
| 2527 | diego  | 20 | 0  | 1730 | 8100 | 5704 | S | 0.1  | 0.2  | 0:00.07  | sshd     |

### Como Interpretar:

- **PID**: Identificador único do processo.
- **USER**: Usuário que iniciou o processo.
- **PR (Priority)**: Prioridade do processo.
- **%CPU**: Percentual de uso da CPU pelo processo.
- **%MEM**: Percentual de uso da memória pelo processo.
- **TIME+**: Tempo total de CPU consumido.
- **COMMAND**: Nome ou comando associado ao processo.

### Filtrando por Usuário:

Durante o uso do top, digite `u` e insira o nome do usuário para visualizar processos apenas daquele usuário.

---

## Listando Processos com ps

**Comando:**

```bash
ps aux
````

Exibe uma lista detalhada de processos no sistema.

### Exemplo de Saída:

| USER     | PID | %CPU | %MEM | VSZ    | RSS   | TTY | STAT | START | TIME  | COMMAND               |
| -------- | --- | ---- | ---- | ------ | ----- | --- | ---- | ----- | ----- | --------------------- |
| root     | 1   | 0.0  | 0.3  | 101584 | 12864 | ?   | Ss   | 08:12 | 00:01 | /sbin/init            |
| root     | 2   | 0.0  | 0.0  | 0      | 0     | ?   | S    | 08:12 | 00:00 | \[kthreadd]           |
| www-data | 225 | 0.0  | 0.1  | 5585   | 5796  | ?   | S    | 12:27 | 00:00 | nginx: worker process |

### Como Interpretar:

* **USER**: Usuário dono do processo.
* **%CPU**: Percentual de uso da CPU.
* **%MEM**: Percentual de uso da memória.
* **VSZ**: Tamanho virtual do processo em KB.
* **RSS**: Tamanho da memória residente em KB.
* **STAT**: Estado do processo (S para sleeping, R para running, etc.).
* **COMMAND**: Comando que iniciou o processo.

---

## Filtrando Processos com Pipe e Grep

**Comando:**

```bash
ps aux | grep nginx
```

Filtra os processos listados pelo comando `ps aux` para mostrar apenas os relacionados ao Nginx.

### Exemplo de Saída:

| USER     | PID  | %CPU | %MEM | VSZ   | RSS   | TTY | STAT | START | TIME  | COMMAND                               |
| -------- | ---- | ---- | ---- | ----- | ----- | --- | ---- | ----- | ----- | ------------------------------------- |
| root     | 2255 | 0.0  | 0.3  | 55180 | 12044 | ?   | S    | 12:27 | 00:00 | nginx: master process /usr/sbin/nginx |
| www-data | 2258 | 0.0  | 0.1  | 55852 | 5796  | ?   | S    | 12:27 | 00:00 | nginx: worker process                 |
| www-data | 2259 | 0.0  | 0.1  | 55852 | 5796  | ?   | S    | 12:27 | 00:00 | nginx: worker process                 |

### Explicação:

Lista os processos relacionados ao Nginx.
O último processo listado pode ser o próprio comando de busca (`grep`). Para evitar isso, usamos:

```bash
ps aux | grep -v grep | grep nginx
```

---

### 16. Scripts de Automação 

*(Conteúdo completo da seção, exatamente como estava no arquivo)*







