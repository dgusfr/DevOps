# Comandos Essenciais do Windows

### **Comandos Fundamentais do CMD**

A seguir, uma lista dos comandos mais utilizados para navegar e gerenciar arquivos e o sistema através da linha de comando.

| Comando | Descrição |
| :--- | :--- |
| **`dir`** | Lista todos os arquivos e pastas no diretório (pasta) atual. |
| **`cd`** | **C**hange **D**irectory (Mudar Diretório). Permite navegar entre as pastas. Ex: `cd Documentos` entra na pasta "Documentos". `cd ..` retorna para a pasta anterior. |
| **`tree`** | Exibe a estrutura de pastas de um diretório de forma gráfica, em formato de árvore. |
| **`type`** | Mostra o conteúdo completo de um arquivo de texto diretamente no terminal. Ex: `type codigo.js`. |
| **`more`** | Funciona de forma semelhante ao `type`, mas exibe o conteúdo de um arquivo página por página, ideal para arquivos longos. Pressione a barra de espaço para avançar. |
| **`copy`** | Copia um ou mais arquivos de um local para outro. Ex: `copy arquivo.txt C:\Backup`. |
| **`move`** | Move um ou mais arquivos de um local para outro. Diferente do `copy`, o arquivo é removido da origem. Também pode ser usado para renomear pastas. |
| **`rename`** | Renomeia um arquivo. Ex: `rename antigo.txt novo.txt`. |
| **`cls`** | **C**lear **S**creen (Limpar Tela). Limpa todas as informações exibidas na janela do terminal. |
| **`fc`** | **F**ile **C**ompare (Comparação de Arquivos). Compara dois arquivos e mostra as diferenças entre eles. Se forem idênticos, informa que não há diferenças. |
| **`systeminfo`** | Exibe informações detalhadas sobre a configuração do seu sistema operacional Windows e hardware. |
| **`rmdir`** | **R**e**m**ove **D**irectory (Remover Diretório). Exclui uma pasta vazia. O comando `del` também pode ser usado para excluir arquivos e, com parâmetros específicos, diretórios. |
| **`shutdown`** | Permite desligar (`shutdown /s`) ou reiniciar (`shutdown /r`) o computador. Pode ser agendado usando parâmetros adicionais. |
| **`date`** | Exibe a data atual do sistema e permite que você a altere. |
| **`find`** | Procura por uma sequência de texto específica dentro de um ou mais arquivos. |
| **`exit`** | Fecha a janela do Prompt de Comando. |

-----

# **Introdução aos Scripts (.bat)**

#### **O que é um script?**

É um arquivo de texto simples, salvo com a extensão **`.bat`**, que contém uma sequência de comandos do CMD. Quando executado, o Windows lê o arquivo e executa os comandos em ordem, um após o outro. Scripts são a base para a automação de tarefas repetitivas.

#### **Como criar um script simples?**

1.  Abra o **Bloco de Notas**.
2.  Digite os comandos que deseja, um por linha.
3.  Vá em `Arquivo > Salvar Como`.
4.  Em "Tipo", selecione "Todos os arquivos".
5.  Dê um nome ao arquivo e termine com a extensão `.bat` (ex: `meu_script.bat`).

**Exemplo:**

```batch
@echo off
echo Ola mundo!
pause
```

#### **Como executar um script?**

1.  Abra o Prompt de Comando (CMD).
2.  Use o comando `cd` para navegar até a pasta onde o script foi salvo.
3.  Digite o nome do script (ex: `meu_script.bat`) e pressione Enter.

#### **Comandos Úteis em Scripts**

| Comando | Função no Script |
| :--- | :--- |
| **`@echo off`** | Usado no início do script para evitar que cada comando seja exibido na tela ao ser executado. Deixa a saída mais limpa. |
| **`echo`** | Exibe uma mensagem de texto na tela. Ex: `echo Iniciando processo...`. |
| **`pause`** | Pausa a execução do script e exibe a mensagem "Pressione qualquer tecla para continuar...". Útil para verificar resultados antes de prosseguir. |

**Exemplo de script para mover arquivos:**

Este script move todos os arquivos com a extensão `.log` do diretório atual para uma subpasta chamada "Backup".

```batch
@echo off
cls
echo Executando o script para mover logs...
move *.log .\Backup
echo Processo finalizado!
pause
```

-----

### **Compactando Arquivos com o Comando `tar` e Scripts**

O comando `tar` (originalmente de sistemas Unix, mas disponível no Windows 10/11) é uma ferramenta poderosa para agrupar e compactar arquivos.

  * **Comando:** `tar`
  * **Função:** Agrupar e compactar arquivos.
  * **Flags Essenciais:**
      * `-c`: **C**riar um novo arquivo compactado.
      * `-f`: Especifica o **f**ile (arquivo) de saída (o nome do arquivo `.zip` ou `.tar`).
      * `-t`: Lis**t**a o conteúdo de um arquivo compactado.

**Exemplo de compactação:**

O comando abaixo agrupa dois arquivos XML em um único arquivo chamado `notas.zip`.

```bash
tar -cf notas.zip NF001.xml NF002.xml
```

#### **Automatizando a Compactação com um Script**

Podemos criar um script `.bat` para compactar todos os arquivos de um determinado tipo (como `.xml`) em uma pasta.

**Script `compactador.bat`:**

```batch
@echo off
echo Compactando todos os arquivos XML...
tar -cf notas.zip *.xml
echo Compactacao concluida. O arquivo notas.zip foi criado.
pause
```

**Como funciona:**

1.  `@echo off`: Limpa a tela, mostrando apenas o resultado dos comandos `echo`.
2.  `echo Compactando...`: Exibe uma mensagem informativa para o usuário.
3.  `tar -cf notas.zip *.xml`: O `*` é um curinga que representa "todos". Assim, o comando `tar` cria um arquivo `notas.zip` contendo **todos os arquivos** que terminam com a extensão `.xml` na pasta atual.
4.  `pause`: Aguarda o usuário pressionar uma tecla antes de fechar a janela, permitindo a leitura da mensagem final.


#### **Tratamento de Erros em Scripts no Windows Prompt (CMD)**

#### **Introdução**

Durante a criação e execução de scripts no Prompt do Windows (CMD), é comum encontrarmos erros. Um script pode falhar se um arquivo não for encontrado, se uma pasta de destino não existir ou por diversos outros motivos. Saber como identificar e tratar esses erros é fundamental para garantir que nossos scripts sejam robustos, confiáveis e que informem o usuário sobre o que está acontecendo.

#### **Cenário Prático: O Script Falha**

Imagine um script `compactador.bat` que deveria compactar todos os arquivos `.xml` de uma pasta. Se não houver nenhum arquivo `.xml` no diretório, o comando `tar` falhará e exibirá uma mensagem de erro no terminal.

**Script `compactador.bat` (Versão Inicial):**

```batch
@echo off
echo Compactando arquivos...
tar -cf notas.zip *.xml
```

**Execução com Falha:**

Se executarmos `.\compactador.bat` em uma pasta sem arquivos `.xml`, o resultado será:

```
Compactando arquivos...
tar: Couldn't visit directory: No such file or directory
tar: Error exit delayed from previous errors.
```

O script termina com uma mensagem de erro que pode não ser clara para o usuário final.

-----

### **Técnicas para Tratamento de Erros**

#### **1. Redirecionando a Saída de Erro**

Uma forma simples de gerenciar erros é evitar que eles poluam a tela. Podemos redirecionar todas as mensagens de erro para um arquivo de log, que pode ser analisado posteriormente. Para isso, usamos o operador `2>`.

**Entendendo os Redirecionadores (Handles)**

No CMD, existem canais de comunicação padrão para os comandos:

  * **`0` (stdin)**: Entrada padrão, geralmente o teclado.
  * **`1` (stdout)**: Saída padrão, as mensagens de sucesso que aparecem no terminal.
  * **`2` (stderr)**: Saída de erro, as mensagens de erro que também aparecem no terminal.

Portanto, `2> erros.txt` significa: "pegue tudo o que for enviado para o canal de erro (`2`) e grave (`>`) no arquivo `erros.txt`".

**Modificando o Script:**

```batch
@echo off
echo Compactando arquivos...
tar -cf notas.zip *.xml 2> erros.txt
```

**Resultado da Execução:**

Agora, ao executar o script, o terminal exibirá apenas:

```
Compactando arquivos...
```

A mensagem de erro não aparece mais na tela. Em vez disso, um arquivo chamado `erros.txt` é criado na mesma pasta. Para ver o erro, usamos o comando `type`:

```
C:\> type erros.txt
tar: Couldn't visit directory: No such file or directory
tar: Error exit delayed from previous errors.
```

#### **2. Verificando o Código de Saída com `%ERRORLEVEL%`**

Redirecionar o erro é útil, mas o script ainda falha silenciosamente. O ideal é que o script **saiba** que um erro ocorreu e **informe** o usuário. Para isso, usamos a variável de sistema `%ERRORLEVEL%`.

  * **`%ERRORLEVEL%`**: Armazena o código de retorno do último comando executado.
      * **`0`**: Sucesso. O comando foi executado sem problemas.
      * **Diferente de `0`**: Falha. Ocorreu algum erro durante a execução.

Podemos usar uma estrutura `IF` para verificar o valor de `%ERRORLEVEL%` imediatamente após o comando crítico. O comparador `NEQ` significa "Não é igual a" (Not Equal).

**Modificando o Script (Versão Final e Robusta):**

```batch
@echo off
echo Compactando arquivos...

REM Tenta executar a compactacao e redireciona a saida de erro
tar -cf notas.zip *.xml 2> erros.txt

REM Verifica se o comando anterior retornou um erro
IF %ERRORLEVEL% NEQ 0 (
  echo.
  echo [AVISO] Ocorreu um erro durante a execucao.
  echo Verifique o arquivo 'erros.txt' para mais detalhes.
) ELSE (
  echo.
  echo Arquivos compactados com sucesso!
)

pause
```

**Resultado da Execução com Falha:**

Agora, quando o script é executado e falha, o usuário recebe um feedback claro e direto:

```
Compactando arquivos...

[AVISO] Ocorreu um erro durante a execucao.
Verifique o arquivo 'erros.txt' para mais detalhes.
Pressione qualquer tecla para continuar. . .
```

---
---

## **Trabalhando com Variáveis em Scripts**

#### **Introdução às Variáveis**

Em scripts, frequentemente precisamos armazenar e manipular informações temporariamente, como um nome de usuário, um caminho de pasta ou um status. Para isso, usamos **variáveis**.

Uma variável é um espaço na memória do computador que recebe um nome e armazena um valor. Esse valor pode ser consultado, utilizado e modificado ao longo da execução do script. O conceito de variáveis é fundamental e utilizado em praticamente todas as linguagens de programação, como Java, Python e, claro, em scripts de shell.

#### **Criando e Usando Variáveis no Prompt**

Você pode criar e testar variáveis diretamente no Prompt de Comando (CMD).

1.  **Abra o CMD** e navegue até um diretório de sua preferência (ex: `cd Desktop`).
2.  **Use o comando `set`** para criar uma variável e atribuir um valor a ela com o sinal de `=`.
      * **Importante:** Não deve haver espaços antes ou depois do sinal `=`.

**Exemplo:**

```batch
C:\Users\SeuUsuario\Desktop>set saudacao=Bem-vindo ao mundo dos scripts!
```

Neste exemplo, criamos a variável `saudacao` e atribuímos o texto "Bem-vindo ao mundo dos scripts\!" a ela.

3.  **Acesse o conteúdo da variável** com o comando `echo`, colocando o nome da variável entre sinais de porcentagem (`%`).

**Exemplo:**

```batch
C:\Users\SeuUsuario\Desktop>echo %saudacao%
```

**Resultado:**

```
Bem-vindo ao mundo dos scripts!
```

> **Observação:** O Windows não diferencia maiúsculas de minúsculas nos nomes das variáveis. Usar `%saudacao%` ou `%SAUDACAO%` produzirá o mesmo resultado.

-----

### **Criando um Script Interativo com Entrada do Usuário**

A verdadeira força das variáveis em scripts aparece quando as usamos para capturar informações do usuário em tempo real.

Vamos criar um script que coleta o nome e o e-mail do usuário e, em seguida, exibe essas informações na tela.

1.  **Abra o Bloco de Notas** (digite `notepad` no CMD e pressione Enter).
2.  **Escreva o script abaixo** no Bloco de Notas.

**Script `coleta_dados.bat`:**

```batch
@echo off
rem Limpa a tela para uma exibicao limpa.
cls

echo Por favor, forneca as seguintes informacoes:
echo.

rem Pede ao usuario para digitar o nome e armazena na variavel 'nome'.
set /p nome=Digite seu nome completo: 

rem Pede ao usuario para digitar o e-mail e armazena na variavel 'email'.
set /p email=Digite seu e-mail principal: 

echo.
echo ..................................................................................
echo OBRIGADO! DADOS COLETADOS:
echo Nome: %nome%
echo E-mail: %email%
echo ..................................................................................

pause
```

#### **Explicação do Script**

| Comando | Função |
| :--- | :--- |
| **`@echo off`** | Desativa a exibição dos comandos. A tela fica mais limpa, mostrando apenas os resultados. |
| **`rem`** | Adiciona um **rem**arque (comentário). O Windows ignora qualquer texto nesta linha, servindo apenas para explicar o código. |
| **`cls`** | Limpa a tela do terminal. |
| **`set /p nome=...`** | O comando `set` com a opção `/p` (de *prompt*) faz duas coisas: 1. Exibe a mensagem à direita do `=`. 2. Pausa o script e aguarda o usuário digitar algo e pressionar Enter. O texto digitado é então armazenado na variável `nome`. |
| **`echo.`** | O comando `echo` seguido de um ponto cria uma linha em branco, útil para organizar visualmente a saída. |
| **`echo Seu nome é %nome%`**| Exibe o texto e substitui `%nome%` pelo valor que o usuário digitou e que foi armazenado na variável. |
| **`pause`** | Pausa a execução e exibe "Pressione qualquer tecla para continuar...", dando tempo para o usuário ler a saída antes que a janela feche. |

#### **Salvar e Executar o Script**

1.  No Bloco de Notas, clique em **Arquivo \> Salvar como...**.
2.  Em "Tipo", selecione "Todos os arquivos (\*.\*)".
3.  Nomeie o arquivo como **`coleta_dados.bat`** e clique em "Salvar".
4.  No Prompt de Comando, certifique-se de que você está na pasta onde salvou o arquivo.
5.  Execute o script digitando `.\coleta_dados.bat` e pressionando Enter.

O script irá rodar, solicitar seu nome e e-mail, e ao final exibirá os dados que você forneceu, demonstrando um uso prático e poderoso de variáveis.

---

## **Configurando o `Path` no Windows para Acessar Scripts de Qualquer Lugar**

#### **Introdução**

Até agora, para executar um script, precisávamos navegar até a pasta onde ele estava salvo. Nesta seção, aprenderemos como configurar o Windows para que nossos scripts se tornem "comandos globais", ou seja, executáveis de qualquer local do terminal, assim como os comandos nativos `cd`, `cls` e `dir`.

Para isso, vamos entender e modificar uma das mais importantes **Variáveis de Ambiente** do sistema: a variável `Path`.

#### **O que são Variáveis de Ambiente e a Variável `Path`?**

**Variáveis de Ambiente** são configurações globais que o sistema operacional e outros programas utilizam. Elas armazenam informações essenciais, como a localização da pasta de perfil do usuário (`%USERPROFILE%`) ou o número de processadores (`%NUMBER_OF_PROCESSORS%`).

A variável **`Path`** é uma das mais importantes. Ela contém uma lista de caminhos de pastas, separados por ponto e vírgula (`;`). Quando você digita um comando no terminal, o Windows procura por um arquivo executável com aquele nome em todas as pastas listadas na variável `Path`, na ordem em que aparecem.

**Visualizando o Conteúdo do `Path`**

Para ver a lista de pastas que o seu sistema já consulta, use o comando `echo %path%`.

```batch
C:\>echo %path%
C:\Windows\system32;C:\Windows;C:\Windows\System32\Wbem;C:\Program Files\nodejs\;...
```

O resultado mostra todos os diretórios onde o Windows busca por comandos. É por isso que `notepad.exe` pode ser executado de qualquer lugar, pois `C:\Windows\system32` está no `Path`.

-----

### **Adicionando seu Script ao `Path` (Passo a Passo)**

Para que o Windows encontre nossos scripts, precisamos adicionar a pasta que os contém à variável `Path`.

#### **Passo 1: Criar uma Pasta Dedicada para Scripts**

É uma boa prática manter todos os seus scripts personalizados em um único local. Vamos criar uma estrutura de pastas para isso.

```batch
C:\Users\SeuUsuario>cd Desktop
C:\Users\SeuUsuario\Desktop>mkdir MeusScripts\bin
```

  * `mkdir MeusScripts\bin`: Cria a pasta `MeusScripts` na Área de Trabalho e, dentro dela, uma subpasta `bin` (abreviação comum para "binários" ou executáveis).

#### **Passo 2: Mover o Script para a Nova Pasta**

Mova o script que você deseja tornar global (ex: `coleta_dados.bat`) para a pasta `bin` recém-criada.

```batch
C:\Users\SeuUsuario\Desktop>move coleta_dados.bat .\MeusScripts\bin
```

#### **Passo 3: Adicionar a Pasta ao `Path` com `setx`**

Usamos o comando `setx` para modificar uma variável de ambiente de forma **persistente** (a alteração sobrevive ao reinício do computador).

```batch
C:\>setx path "%path%;C:\Users\SeuUsuario\Desktop\MeusScripts\bin"
```

**Analisando o comando:**

  * `setx path`: Define que queremos alterar permanentemente a variável `path`.
  * `"%path%;..."`: Esta é a parte crucial.
      * `%path%`: Pega todo o conteúdo atual da variável `Path`.
      * `;`: Adiciona um ponto e vírgula para separar o caminho antigo do novo.
      * `C:\Users\SeuUsuario\Desktop\MeusScripts\bin`: Adiciona o caminho completo da nossa pasta de scripts ao final da lista.
      * As aspas duplas (`"`) garantem que o comando funcione mesmo que os caminhos contenham espaços.

> **Importante:** Para que a alteração tenha efeito, **você deve fechar e abrir novamente o Prompt de Comando.**

#### **Resolvendo Problemas de Permissão (Erro de Acesso Negado)**

Ao tentar alterar o `Path`, você pode usar o parâmetro `/M` para aplicar a mudança a todos os usuários do sistema (`setx path "..." /M`). No entanto, isso pode gerar um erro de "Acesso ao caminho do Registro negado".

  * **Causa:** Você está tentando modificar uma configuração de nível de sistema com permissões de um usuário comum.
  * **Solução:** Abra o Prompt de Comando **como administrador**. Para fazer isso, pesquise por `cmd` no Menu Iniciar, clique com o botão direito sobre ele e selecione "Executar como administrador". Em seguida, execute o comando `setx` novamente.

#### **Passo 4: Testando o Script Global**

Após adicionar o caminho ao `Path` e reiniciar o terminal, você pode executar seu script de qualquer lugar, digitando apenas o nome dele.

```batch
C:\>coleta_dados.bat
Por favor, forneca as seguintes informacoes:
...
```

Funcionou\! Agora seu script é tão acessível quanto qualquer outro comando do Windows.

-----

### **Resumo dos Comandos Utilizados**

| Comando | Descrição |
| :--- | :--- |
| **`echo %path%`** | Exibe o conteúdo atual da variável de ambiente `Path`. |
| **`mkdir`** | Cria um novo diretório (pasta). |
| **`move`** | Move arquivos ou diretórios de um local para outro. |
| **`setx`** | Cria ou modifica variáveis de ambiente de forma permanente para o usuário ou para o sistema. |

