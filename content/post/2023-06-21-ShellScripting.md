---
title: Shell Scripting
date: 2023-06-21
categories: [programming, bash]
tags: [bash, coding, "shell scripting", programming]
---

> Artigo ainda em construção, releve os erros. Talvez algumas informações estejam incompletas
> {: .prompt-warning }

# <img src="/assets/img/posts_img/full_colored_light.png">

### Arquivos .sh

Exitem diversos shells, alguns exemplos são `sh`, `fish`, `zsh`, `tcsh`, cada um com suas peculiaridades, porém o mais difundido é o `bash`. Esse será a base para os conhecimentos compartilhados nesse artigo.

No inicio de um arquivo **.sh** é comum adicionarmos um comentário chamado de "shabang" para dizermos qual interpretador shell deve executar o programa

```bash
#!/bin/bash
```

Um exemplo utilizando outro shell

```bash
#!/bin/fish
```

Ao criar um programa .sh precisamos dar permissão de execução do sistema. No Linux fazemos isso com `chmod`

```sh
$ chmod +x file.sh
```

Parâmetro +x adiciona permissão de execução do sistema ao arquivo.

Da mesma forma que a primeira linha comentada do programa diz qual shell deve executar o arquivo, é possível passar essa informação diretamente por linha de comando. Exemplo:

```bash
$ bash ./file.sh
```

Dessa forma, você força qual shell irá executar o programa o arquivo não precisará ter permissão de execução, evitando assim adicionar esse atributo com `chmod +x`

## VARIÁVEIS

Exemplos:

ENV
PATH
HOME
USERNAME
TERM
RANDOM

### Array de ambiente

```bash
testearray=(laranja maça morango)
```

```sh
echo ${testearray[n]}
```

## Arquivo de configuração

```sh
./bashrc
```

Aqui você pode configurar seu terminal, criando alias, variáveis e funções por exemplo:

```sh
# No final do arquivo, adicionei uma função que criar um diretório e entra no mesmo

function mkcd() {
	mkdir -p $1
	cd $1
}
```

### Cálculos matemáticos

Sintaxe para receber valores matemáticos:

```sh
var=$[ operação ]
```

```sh
var1=$[ 3 + 6 ]

echo $var1 # output: 9
```

##### Exemplo que faz um pull em cada para repositório em uma pasta

```bash
#... abreviado

cd "$gitDir"
numLines="$(find . -type d  -maxdepth 1 | wc -l)"
counter=1

for archive in `find $gitDir -mindepth 1 -maxdepth 1 -type d`
do
#... abreviado
counter=$((counter+1))
percent=$(($counter*100/$numLines)) # Aqui executa a conta de porcentagem. Quantos repositórios já foram atualizados
echo -e "\033[032m[$percent%] Completo\033[m\n"

```

Outra opção é usar o programa `bc`

<img src="/assets/img/posts_img/print_bc.png">

### Condicional

```bash
if comando
then
	comandos
fi

#ou

if comando; then
	comandos
else
	comandos
fi



if comando1; then
	comandos
elif comando2
then
	comandos processameto
elif comando3; then
	comandos processamento
fi

```

```bash
#!/bin/bash

file=$HOME/.bashrc
if [ $USER = daniel ] && [ -w file ]
then
	echo "$USER tem permição para alterar aquivo $file"
else
	echo "$USER não pode alterar $file"
fi
```

### Test

```zsh
if test CONDIÇÃO
then
	comandos
fi

# ou

if [ CONDIÇÃO ]
then
	comandos
fi

# Exemplo

test 1 = 1; echo $?
#output 0
```

#### Parâmetros para comparação que podem ser usados com `if` e/ou `test`

Referência [https://www.vivaolinux.com.br/dica/Conhecendo-o-test](https://www.vivaolinux.com.br/dica/Conhecendo-o-test){:target="\_blank"}

- -b arquivo - Verdadeiro se o arquivo é um block device
- -c arquivo - Verdadeiro se o arquivo é um character device.
- -d arquivo - Verdadeiro se o arquivo é um diretório.
- -e arquivo - Verdadeiro se o arquivo existe.
- f1 -ef f2 - (equivalent file) Verdadeiro se o arquivo f1 é um hard-link para f2.
- n1 -eq n2 - (equal) Verdadeiro se n1 for igual a n2.
- -f arquivo - Verdadeiro se o arquivo existe e é um arquivo regular.
- n1 -ge n2 - (greater than or equal) Verdadeiro se n1 maior ou igual a n2.
- n1 -gt n2 - (greater than) Verdadeiro se n1 for maior que n2.
- -g arquivo - Verdadeiro se o SGID estiver ativado.
- -G file - Verdadeiro se o arquivo (efetivamente) pertencer ao grupo sob qual o script estiver rodando.
- -h arquivo - Verdadeiro de o arquivo for um link simbólico.
  -k arquivo - Verdadeiro se o arquivo tiver a permissão sticky bit ativada.
- n1 -le n2 - (less than or equal) Verdadeiro se n1 menor ou igual a n2.
- n1 -lt n2 - (less than) Verdadeiro se n1 menor que n2.
- -n s (or just s)-(not null) Verdadeiro se a string não for vazia ou nula.
- -N arquivo - Verdadeiro se o arquivo tiver novo conteúdo (desde a última vez que foi lido).
- n1 -ne n2 - (not equal) Verdadeiro se n1 não for igual a n2.
- -O file - Verdadeiro se o arquivo pertencer ao usuário sob o qual o script estiver rodando.
- f1 -nt f2 (newer than) Verdadeiro se o arquivo f1 for mais recente que f1.
- f1 -ot f2 (older than) Verdadeiro se o arquivo f1 for mais antigo que f1.
- -p file - Verdadeiro se o arquivo for um pipe.
- -r file - Verdadeiro se um arquivo for lido (pelo seu script).
- -s arquivo - Verdadeiro se o arquivo existe e não estiver vazio.
- -S arquivo - Verdadeiro se o arquivo for um socket.
- -t fd - Verdadeiro se o descritor do arquivo estiver aberto em um terminal.
- -u arquivo - Verdadeiro se o arquivo tiver set-user-id (suid) ativada.
- -w arquivo - Verdadeiro se o arquivo tiver permissão de escrita (pelo seu script).
- -x arquivo - Verdadeiro se o arquivo for executável.
- -z s - (zero length) Verdadeiro se a string for igual a zero ou não possuir nenhum valor atribuído. Ex. variável nula ou arquivo vazio.

Outros operadores mais comuns

- s1 = s2 - Verdadeiro se a string s1 é igual a s2.
- s1 != s2 - Verdadeiro se a string s1 for diferente de s2.
- s1 < s2 - Verdadeiro se a string s1 for menor que s2.
- s1 > s2 - Verdadeiro se a string s1 for maior que s2.

Pode-se usar a palavra `test` para testar um comando,

- OBS: O comando `test` não suporta números float

**Todas essas informações podem ser encontradas com o comando `man bash`** e pesquisar esses operadores digitando `/` e digitando `CONDITIONAL EXPRESSIONS`

Exemplos

```zsh

#!/bin/bash

n1=10
n2=12

if test $n1 -eq $n2
then
     echo "$n1 = $n2"
elif [ $n1 -gt $n2 ]; then
	echo "$n1 > $n2"
elif [ $n1 -lt $n2 ]
then
    echo "$n1 < $n2"
else
    echo "erro"
fi
```

### Comparação de arquivos no comando teste

```bash
test -f ~/.bashrc ; echo $?
#output: 0
```

### Comando case

```bash
#!/bin/bash

case <variavel> in
	valor)
		comando1;;
	valor2)
		comandos2;;
	*)
		comandos-padrão;;
esac
```

## Estrutura de repetição

for, while, until

```bash
for valor in lista
do
	comandos
done
```

exemplo

```bash
 #!/bin/zsh

lista=("Laranja" Maçã mamão morango "leite condensado")
n=1

for item in $lista
do
    echo $n " - " $item
    n=$[ $n + 1 ] #ou n=$(($n+1))
done

```

`for` estilo C

```bash

for ((i = 0; i < $n; i++))
do
	# processamento
done

#também é possível usar wildcards (caracteres curingas) para criar um for

for i in {1..10}; do
	echo $i
done
```

Loop com `until`

```bash
echo "loop com until"

counter=0
until [ $counter -gt 5 ] # o loop executa então não satisfaze a condição
do
  echo "Count: $counter"
  ((counter++))
done

echo "loop com while"
#no mesmo funcionamento com while
counter=0

while [ $counter -le 5 ]; do
	echo "Count: $counter"
	((counter++))
done
```

Para efeito de demostração, um código `fish shell` que funcione da mesma forma que o código anterior

```sh
#!/bin/fish

set counter 0
while ! [ $counter -gt 5 ]
  echo "Count: $counter"
  set counter (math $counter + 1)
end

echo -e "\n"

set counter 0

while [ $counter -le 5 ];
	echo "Count: $counter"
	set counter (math $counter + 1 )
end

```

## Parâmetros de linha de comando

./ex.sh param1 param2 ....
$0 $1 $2

$# - numero de parametros

#### Shift

reduz ou passa o parametro para uma variável anterior, ex, valor de $3 passa para => $2

##### Comando read

```bash
read nome -p "nome: "
echo $nome
```

### whiptail 
Caixa de dialogo

```bash
whiptail --title "Caixa" --msgbox "caixa" --fb 10 50
```

## funções

```bash
function nome_função {
	comandos
}

#ou

nome_funcao(){
	comandos
}

```

#### Recomendações

**English**

- [60 Linux Commands you NEED to know (in 10 minutes)](https://youtu.be/gd7BXuUQ91w?si=JAsB6F2310c8t_ps)

- [You need to learn BASH Scripting RIGHT NOW!! - NetworkChuck](https://www.youtube.com/playlist?list=PLIhvC56v63IKioClkSNDjW7iz-6TFvLwS)

- [The 50 Most Popular Linux & Terminal Commands - Full Course for Beginners](https://youtu.be/ZtqBQ68cfJc?si=0s6wR5sICExGjBXt)

- [Bash Scripting Tutorial for Beginners](https://youtu.be/tK9Oc6AEnR4?si=eAo6BqWEgINXJTyi)

- [the Linux File System explained in 1,233 seconds](https://youtu.be/A3G-3hp88mo?si=NXIHiYDammdEJzRE)

**Português**

[O que é Shell? (Software) - Diolinux Explica](https://youtu.be/zskFbzsZu8s?si=oKA6ncYc-BpNj-d6)
[Curso de Shell Scripting - Programação no Linux](https://youtube.com/playlist?list=PLucm8g_ezqNrYgjXC8_CgbvHbvI7dDfhs&si=2ObT0BSwmupIPy8_)
[30 Comandos do Terminal Linux BÁSICOS](https://youtu.be/QZ2nyxzZXPY?si=D67tZvJN2gY2SY7Z)
