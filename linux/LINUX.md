# Linux

## Comandos GNU e Unix

### Shell

Shell é uma interface de usuário utilizada para acessar os recursos administrados pelo Sistema Operacional (SO). Também é um ambiente de programação que executa comandos externos e sequência de comandos (script), assim como pode utilizar variáveis e variáveis de ambiente. Essa interface é chamada *shell* (em português, "casca" ou "concha") porque é a camada mais externa ao kernel do SO.

#### Bourne Shell

Desenvolvido por Stephen Bourne em 1979, Bourne Shell (sh) é o shell padrão para Unix e é a matriz de outros shells para sistemas Unix-like.

#### Bourne-Again Shell

Desenvolvido por Brian Fox para o projeto GNU em 1989, Bourne-Again Shell (bash) é uma evolução do Bourne Shell e é o shell mais utilizado por sistemas Linux devido à sua portabilidade, pois possui características encontradas em outros "sabores" de shell. Atualmente, o bash é o shell coberto pela certificação Linux LPIC-1.

### Variáveis de ambiente

| Variável | Dado contido |
| --- | --- |
| `$SHELL` | Caminho absoluto do shell em execução |
| `$PATH` | Lista de diretórios dos programas externos em execução no sistema |

### Comandos internos e externos

Comandos internos são os comandos embutidos no shell. Comandos externos pertencem a programas externos instalados no sistema.

### Principais comandos internos

#### `echo`

O comando `echo` (em português, "ecoar") retorna o dado informado na saída padrão do sistema.

Exemplo:

Retornando o caminho absoluto do shell em execução, que é o bash neste caso:

```bash
user@linux:~$ echo $SHELL
/bin/bash
```

#### `type`

O comando `type` (em português, "tipo") retorna uma mensagem indicando a origem do comando informado. Se esse comando é interno, `type` retorna uma mensagem com essa afirmação. Caso seja externo, retorna uma mensagem com o caminho absoluto desse comando. Se esse já foi utilizado durante a sessão atual, então estará carregado no cache interno do sistema e isso também será exibido junto com seu caminho.

Exemplos:

Retornando mensagem de que o comando `echo` é interno do shell:

```bash
user@linux:~$ type echo
echo is a shell builtin
```

Retornando mensagem com o caminho absoluto de `tar`, por ser um comando externo:

```bash
user@linux:~$ type tar
tar is /bin/tar
```

Retornando mensagem com a informação de que `clear` está no cache interno e seu caminho absoluto:

```bash
user@linux:~$ type clear
clear is hashed (/usr/bin/clear)
```

#### `pwd`

O comando `pwd` - Print Working Directory (em português, "Imprimir Diretório de Trabalho") - retorna o caminho absoluto do diretório atual.

Exemplo:

Retornando o caminho absoluto do diretório atual, que neste caso é o diretório do usuário atual:

```bash
user@linux:~$ pwd
/home/user
```

#### `cd`

O comando `cd` - Change Directory (em português, "Mudar Diretório") - muda o diretório de trabalho atual para o diretório informado.

Exemplos:

Mudando para `/user/local/` utilizando seu caminho absoluto:

```bash
user@linux:/etc/apt$ cd /usr/local/
user@linux:/usr/local$
```

Mudando para `/usr/local/share/applications/` utilizando um caminho relativo:

```bash
user@linux:/usr/local$ cd share/applications/
user@linux:/usr/local/share/applications$
```

Mudando para o diretório pai utilizando o nome especial "`..`" (ponto ponto):

```bash
user@linux:/usr/local/share/applications$ cd ../
user@linux:/usr/local/share$
```

Mudando para `/usr/` concatenando "`..`" (ponto ponto) para aproveitar a árvore do diretório atual:

```bash
user@linux:/usr/local/share/applications$ cd ../../
user@linux:/usr/local$
```

Mudando para `/usr/local/games` concatenando "`..`" (ponto ponto) com `games/` para aproveitar a árvore do diretório atual:

```bash
user@linux:/usr/local/share/applications$ cd ../../games/
user@linux:/usr/local/games$
```

> Presente apenas em subdiretórios, o nome especial "`..`" (ponto ponto) aponta para o diretório pai do próprio subdiretório.

Mudando para o diretório pessoal do usuário atual utilizando o atalho "`~`" (til):

```bash
user@linux:/usr/local/share/applications$ cd ~/
user@linux:~$
```

Mudando para o diretório "`Desktop/`" do usuário atual utilizando o atalho "`~`" (til):

```bash
user@linux:/usr/local/share/applications$ cd ~/Desktop/
user@linux:~/Desktop$
```

Alternando entre o diretório atual e último acessado:

```bash
user@linux:/etc/apt$ cd /usr/local/
user@linux:/usr/local$ cd -
/etc/apt
user@linux:/etc/apt$ cd -
/usr/local
user@linux:/usr/local$
```

#### `ls`

O comando `ls` - abreviação de list (em português, "listar") - retorna uma lista com o conteúdo do diretório atual ou de um diretório informado.

Exemplos:

Listando o conteúdo do diretório atual:

```bash
user@linux:/usr$ ls
game  lib  sh  sources
```

Listando o conteúdo do diretório `/usr/game/` por um caminho relativo:

```bash
user@linux:/usr$ ls game/
bkp  old  sgames
```

### Scripts

Para executar um comando ou script que não é interno e não está nos diretórios listados no `$PATH`, é necessário inserir seu caminho absoluto ou o caminho relativo junto ao nome do arquivo. Caso o script esteja no diretório atual, é possível utilizar o nome especial "`.`" (ponto) seguido do nome do arquivo.

Exemplos:

> Script de exemplo: `/home/user/Desktop/test.sh`

```bash
# Exibir data atual
echo -en '\nDATA ATUAL: ' && date
#
# Exibir diretório atual
echo -en 'DIRETÓRIO ATUAL: '$(pwd)'\n\n'
```

Executando o script utilizando seu caminho absoluto:

```bash
user@linux:~$ /home/user/Desktop/test.sh

DATA ATUAL: Sat 04 Apr 2020 10:17:04 PM -03
DIRETÓRIO ATUAL: /home/user

user@linux:~$
```

Executando o script utilizando um caminho relativo:

```bash
user@linux:~$ Desktop/test.sh

DATA ATUAL: Sat 04 Apr 2020 10:17:27 PM -03
DIRETÓRIO ATUAL: /home/user

user@linux:~$
```

Executando o script em seu diretório utilizando o nome especial "`.`" (ponto):

```bash
user@linux:~/Desktop$ ./test.sh

DATA ATUAL: Sat 04 Apr 2020 10:17:49 PM -03
DIRETÓRIO ATUAL: /home/user/Desktop

user@linux:~/Desktop$
```

> Presente em todos os diretórios, o nome especial "`.`" (ponto) aponta para o próprio diretório.

Ao tentar executar o script direto, sem indicar para o shell que esse está no diretório atual, ocorre um erro pois, por padrão, o shell procura o script nos diretórios listados no `$PATCH`:

```bash
user@linux:~/Desktop$ test.sh
bash: test.sh: command not found
```
