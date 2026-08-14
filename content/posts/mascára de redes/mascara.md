+++
date = '2026-07-22T19:24:50-03:00'
draft = false
title = 'Como isso funciona? - Máscara de rede, CIDR e VLSM'
+++

###  Máscara de Rede
A máscara de rede é uma ferramenta que possibilita identificação, segmentação de redes e limitação de número de máquinas na rede.
Uma máscara é composta por quatro octetos de bits separados por um ".", mas para melhor visualização o padrão é em forma decimal igualmente ao IPv4.

![image](/images/mask1.png)

Você deve estar se perguntando, por que tem octetos que apenas tem "1" e octetos que apenas tem "0". Cada binário tem sua função, informar se o
bit que está na mesma posição do endereço IPv4 vai servir para identificar a rede ou o host, o "1" vai ser lido para identificar a rede, já o 
"0" serve para identificar os host que seriam os aparelhos conectados na rede. Um exemplo abaixo.

![image](/images/mask2.png)

Comparando os bits de IP e máscara é possível visualizar que os 3 primeiros octetos são destinados a identificar a rede, e o último octeto serve 
para identificar os hosts. Sendo assim o endereço IP 192.168.64.0 com a máscara 255.255.255.0, apenas o único octeto que vai mudar vai ser o último, 
podendo ter endereços IP 192.168.64.x, o x pode ser de 1 a 254.



#### Cuidado
Mas não se engane, nunca vai haver uma máscara em que o primeiro octeto começa com 0 e nem qualquer mistura de 1 e 0, sempre a identificação de
rede vem primeiro e em seguida vem a identificação de host.

![image](/images/mask3.png)

### Classes
Ademais a isso na [RFC 791 (Request For Comment)](https://datatracker.ietf.org/doc/html/rfc791), onde é a documentação sobre o IPV4, é citado o
funcionamento da máscara de rede de forma breve, junto a isso foi determinado classes para facilitar qual era a máscara utilizada em determinado IP
e quantos hosts cabem naquela rede apenas olhando o primeiro octeto do IPV4.

|Classes|1°octeto|faixa em decimal|Máscara|Quantidade de hosts|
|-------|--------|----------------|-------|-------------------|
|A|0xxxxxxx|0-127|255.0.0.0|16.777.214|
|B|10xxxxxx|128-191|255.255.0.0|65.534|
|C|110xxxxx|192-223|255.255.255.0|254|

> [!NOTE]
Existem também as classes D e E, mas não serão abordadas nesse post. Junto a isso também existem certos IP's reservados onde esse sistema de
classe não se aplica.

### CIDR (Classless Inter-Domain Routing)
Posteriormente veio o CIDR (Classless Inter-Domain Routing), é um novo método que substitui as classes, agora sendo mais flexível e suportando sub-redes.

sua notação é simples sendo um "/" juntamente com a quantidade de números de identificação de rede, exemplo /24 é a mesma coisa que 255.255.255.0
sendo comum encontrar o número de IP junto ao CIDR 192.32.12.0/24.

|CIDR|binário|
|-------|----|
|/24|11111111.11111111.11111111.00000000|
|/26|11111111.11111111.11111111.11000000|
|/20|11111111.11111111.11110000.00000000|

### VLSM (Variable Length Subnet Masking)
VLSM é uma técnica de segmentação de redes, possibilitando a criação de sub-redes, evitando o desperdício de IP. 

Funciona da seguinte maneira: imagine que fomos contratados para configurar a rede de uma loja e temos a seguinte situação, precisamos configurar a rede
IPv4 149.123.42.0/24, porém o cliente gostaria que tivesse duas redes diferentes, uma rede interna e outra rede com acesso à internet para clientes.
Antes iríamos usar dois IPv4 diferentes, mas com o VLSM nós adicionamos mais um bit de identificação, dividindo a rede em duas redes diferentes.

Isso acontece porque, quando adicionamos esse bit de identificação na máscara de rede, o bit correspondente do IPv4 pode ser 0 ou 1, e podemos ir 
adicionando os bits de identificação para suprir a quantidade de hosts necessários, onde o máximo é /31, porém ninguém consegue se conectar
nessa rede /31, pois por ele só tem um bit de host ele só tem duas possibilidades, porém qualquer rede precisa de broadcast e o ip da rede em si,
sendo assim os dois ips disponíveis estarão ocupados. O máximo não é /32 pois seria apenas um IP de identificação e nenhum host ou IP reservado
conseguiria se conectar na rede.

![image](/images/mask4.png)

O VLSM tem a seguinte regra: do maior número de hosts para o menor número de hosts. Então, quando for fazer várias sub-redes, fique atento a 
quais redes precisam de um número maior de hosts.
