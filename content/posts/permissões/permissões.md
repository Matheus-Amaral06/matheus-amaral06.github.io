+++
date = '2026-08-21T21:17:11-03:00'
draft = false
title = 'Como isso funciona? - Permissões no Linux'
+++
## Primeiro contato
Ao utilizar o comando `ls -l` você já deve ter se departamento com isso.
```bash
$ls -l | awk '{print $1}'

total
-rw-r--r--
drwxr-xr-x
drwxr-xr-x
drwxr-xr-x
drwx------
drwxr-xr-x
drwxr-xr-x
lrwxrwxrwx
-rw-r--r--
```
E você deve estar se perguntando o que é esse monte de letras. Bem, essas são as permissões e o tipo de arquivo. Os tipos de arquivo são o primeiro caractere, e logo em seguida vêm as permissões.

## Tipos e categorias
As permissões tem 3 tipos e 3 categorias
### Tipos
* r - leitura de conteúdo de arquivos ou diretório
* w - escrever conteúdo dentro de arquivos ou diretórios
* x - execução de arquivo ou diretório
* '-' - O hífen quer dizer a respectiva permissão esta desativada
  
### Categorias
* User - Dono do arquivo ou do diretório
* Group - Grupo do arquivo ou diretório
* Others - Outros usuários que não pertecem as outras categorias

No comando `ls -l` conseguimos ver que tem 9 caracteres, e são divididos em 3 categorias, ou seja, os primeiros 3 caracteres são as permissões de user, em seguida vem o grupo e posteriormente others. Essa sequência nunca vai mudar, e igualmente a sequência que vem a permissão 'r', depois 'w' e, por fim, 'x', e também nunca irá mudar.

## Entendimento

As permissões são definidas por binários; o bit '0' significa que a permissão não está ativa, já o bit '1' significa que a permissão está ativa. Como temos três tipos de permissões, vamos ter um conjunto de três bits. '000' é o mínimo, em que está tudo desativado, e '111' é o máximo, em que está tudo ativado. Trazendo isso para a base octal, é fácil dizer que 0 = 000 em base dois, e 7 = 111 em base dois, e constantemente vai se deparar com algo assim, 644 ou 777. Cada dígito desse número de 3 dígitos define a permissão de uma categoria, e, por padrão, ele segue a ordem: o primeiro dígito define as permissões de usuário, o segundo define permissões de grupo e o terceiro define a permissão de outros.

Com isso em mente, fica fácil de entender como funciona o umask. Esse comando carrega um valor octal que, por padrão, é '022'. O seu trabalho é subtrair com o valor padrão que define as permissões ao criar um arquivo ou pasta. Os arquivos têm atribuídos a eles o valor 666 por padrão e os diretórios têm 777 por padrão. Segue um exemplo:

```markdown
Arquivos
666 - 22 = 644
	User
	6 = 110 = rw-
	
	Group
	4 = 100 = r--
	
	Others
	4 = 100 = r--
```

```markdown
Diretório
777 - 22 = 755
	User
	7 = 111 = rwx
	
	Group
	5 = 101 = r-x
	
	Others
	5 = 101 = r-x
```

## Alteração de permissão
A alteração de permissão é feita por um comando chamado `chmod` e tem duas formas de fazer essas alterações.

Primeira forma é utilizar o esquema, inicial da categoria, mais o sinal de '+' para adicionar ou sinal de '-' para remover e a inicial de qual permissão gostaria de alterar.

```markdown
chmod [_OPTION_]... _MODE_[_,MODE_]... _FILE_...
chmod -v u+r <arquivo ou pasta>
```

Segunda forma é utlizar os números em octal.
```markdown    
chmod [_OPTION_]... _OCTAL-MODE FILE_...
chmod -v 644 <arquivo ou pasta>
```

> [!NOTE]
>Tente brincar com as permisões em arquivos e pastas criados para fazer esse teste.

## Conclusão
Com o entendimento sobre permissões você consegue resolver alguns problemas que irá enfrenta na sua jornada no sistema linux, porém tome cuidado permissões para usuários e arquivos errados pode causar problemas graves no seu sistema.
