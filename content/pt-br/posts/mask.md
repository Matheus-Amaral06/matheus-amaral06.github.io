+++
date = '2026-05-26T19:24:50-03:00'
draft = true
title = 'Como isso funciona? - Máscara de rede e CIDR'
+++

### Máscara de rede
A máscara de redes é um conjunto de 4 octetos de bits, ou seja 32 bits ou 4 bytes, onde o seu trabalho é identificar qual parte do seu ip é rede e qual é host, ela identifica bit de rede como 1 e bit de host
como 0, onde o 1 sempre vem primeiro e os bits 1 e 0 não se misturam.

```markdown


11111111.11111111.11111111.00000000 certo
|_________________________|________|
            |                  |
          REDE               HOST

11111111.11111111.00000000.00000000 certo
|________________|_________________|
        |                 |
      REDE               HOST



01111111.11101101.10010111.00100100 errado

11100011.11101101.11110100.10111100 errado
```

Ela foi documentada a primeira vez na [RFC 791 (Request For Comment)](https://datatracker.ietf.org/doc/html/rfc791 "RFC 791") Determinando classes apartir do primeiro octeto do endereço IP
que definem quantos bits terá a sua máscara de rede.

#### Classe A
Todos os IP's da classe A começam com 0, os IP's podem ser de 0-127.x.x.x na forma decimal, essa classe determina a máscara 255.0.0.0, para descobrirmos a quantidade de hosts 
fazemos o calculo 2²⁴ - 2 = 16.777.214
> [!NOTE]
    Utilizamos a base 2 pois só tem possibilidade de ser 1 ou 0 por ser binário, e elevado a 24 pois é a quantidade de bits que podem ser hosts, e menos dois pois um ip é o ip da rede e o outro
    é do broadcast. Segue a mesma lógica para todas as classes
```markdown

255.0.0.0
11111111.00000000.00000000.00000000 
|_______|_________________________|
    |                | 
   REDE             HOST

0-127
0xxxxxxx.xxxxxxxx.xxxxxxxx.xxxxxxxx
```

#### Classe B
Na Classe B os 2 primeiros bits são 1 e em sequencia 0, os IP vão de 128-191, essa classe determina a máscara 255.255.0.0, tendo um total de 65.536 hosts

```markdown

255.255.0.0
11111111.11111111.00000000.00000000 
|________________|_________________|       
        |                 |                
      REDE               HOST              

IP's 
128-191
10xxxxxx.xxxxxxxx.xxxxxxxx.xxxxxxxx
```
#### Classe C
Na Classe C os 3 primeiros bits são alterados ficando "110", essa classe determina a máscara 255.255.255.0, tendo um total de 254 hosts

```markdown

255.255.255.0
11111111.11111111.11111111.00000000 
|_________________________|________|
            |                  |           
          REDE               HOST          

IP's 
192-224
110xxxxx.xxxxxxxx.xxxxxxxx.xxxxxxxx
```
>[!NOTE]
    Existem também classe D e E, mas não seram tratadas nesse post para não prejudicar o racíonio.

|Classe|Binário|Decimal|Quantidade de Hosts|
|------|-------|-------|-------------------|
|A|0xxxxxxx|0-127.x.x.x|16.777.214|
|B|10xxxxxx|128-191.x.x.x|65.536|
|C|110xxxxx|192-224.x.x.x|254|

### Máscaras de subrede
Máscaras de subrede veio com o intuito de economizar ip, reaproveitando um ip para fazer a segmentação da rede. Ela funciona da seguinte forma, digamos que temos que construir uma rede segmentada com apenas
um ip, Esse IP é 192.168.29.0 com a máscara 255.255.255.0 ou seja classe C, temos que fazer uma rede para cada setor 

|Setor|Quantidade de hosts|
|------|-------|
|Contabilidade|55|
|RH|27|
|TI|10|

Com isso em mente temos um problema, como vamos dividir a rede e conseguir aproveitar o ip, a máscara de subrede veio para solucionar esse problema, onde aumentamos o número de bits que reconehcem a rede,
e dividindo a rede em dois a cada vez que aumenta um número de idenficaçao de rede. Para exemplificar irei mostrar como ficaria o ultimo octeto

```markdown
                    Original
                    00000000
                    |______|
                        |
                        +1 bit de rede
                --------|------------
                |                    |
            00000000                10000000
            ^                       ^

```
O primeiro bit do octeto agora virou bit de identicaçao de rede, nesse cenário então a rede foi divida em dois, pois temos a rede que começa com 0 e a rede que começa com um, sendo distintas
apesar de terem o mesmo ip, porém não possuem a mesma máscara, em outras palavra agora existe uma rede 192.168.29.0 e outra 192.168.29.128

