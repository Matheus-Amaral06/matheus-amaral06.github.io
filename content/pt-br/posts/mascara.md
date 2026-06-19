+++
date = '2026-05-26T19:24:50-03:00'
draft = false
title = 'Como isso funciona? - Máscara de rede e CIDR'
+++

### O que é?
A mácara de rede é uma ferramenta que possibilita identificação, segmentação de redes e limitação de número de máquinas na rede. Uma mácara é 
composta por quatro octetos de bits separados por um ".", mas para melhor visualisação o padrão é em forma decimal igualmente ao IPv4. 

'''markdown
Decimal : 255.255.255.0 <- Forma mais comum
Binário : 11111111.11111111.11111111.00000000 <- Forma que o computador interpreta 

'''

Você deve estar se perguntando, por que tem octetos que apenas tem "1" e octetos que apenas tem "0". Cada binário tem sua função, informar se o 
bit que está na mesma poscição do endereço IPv4 vai servir para identificar a rede ou o host, o "1" vai ser lido para identificar a rede, já o "0"
serve para identifficar os host que seriam os aparelhos conectados na rede. Um exemplo abaixo.

'''markdown
IP : 192.168.64.0
Máscara : 255.255.255.0

Agora os mesmos números em binário para melhor compreensão

IP : 11000000.10101000.01000000.00000000
    |__________________________|________|
                  |                 |
                REDE              Host
Máscara : 11111111.11111111.11111111.00000000

'''
Comparando os bits de IP e máscara é possível visualizar que os 3 primeiros octetos são destinados a identificar a rede, e o ultimo octeto serve
para identificar os hosts. Sendo assim o endereço IP 192.168.64.0 com a máscara 255.255.255.0 os único octeto que vai mudar para vai ser o ultimo,
podendo ter endereços IP 192.168.64.103 em alguma máquina.




Mas não se engane nunca vai haver uma máscara em que o primeiro octeto começa com 0 e nem qualquer misturar de 1 e 0, sempre a identificação de rede
vem primeiro e em seguida vem a identificação de host.

'''markdown

Errado : 00000000.00000000.11111111.11111111

Errado : 11101010.00011000.11110001.11000010

Certo  : 11111111.00000000.00000000.00000000
'''

Ademais a isso na [RFC 791 (Request For Comment)](https://datatracker.ietf.org/doc/html/rfc791), onde é a documentação sobre o IPV4, é citado o
funcionamento da máscara de rede de forma breve, junto a isso foi determinado classes para facilitar qual era a máscara utilizada e quantos hosts
cabem naquela rede apenas olhando o primeiro octeto do IPV4.

|Classes|1°octeto|faixa em decimal|Máscara|Quantidade de hosts|
|-------|--------|----------------|-------|-------------------|
|A|0xxxxxxx|0-127|255.0.0.0|16.777.214|
|B|10xxxxxx|128-191|255.255.0.0|65.534|
|C|110xxxxx|192-223|255.255.255.0|254|

> [!NOTE]
a

