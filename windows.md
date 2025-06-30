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

### **Introdução aos Scripts (.bat)**

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

A criação de scripts como este agiliza o trabalho, automatiza rotinas e facilita a manutenção de sistemas, tornando-se uma habilidade fundamental para qualquer usuário avançado ou profissional de TI.