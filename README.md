# Banco de Dados Redis

### Instalação e Utilização
instalar o Redis no docker com o comando ```docker pull redis``` , e depois de executar utilizar o comando ```redis-cli```

## CRUD de registros

#### Adicionar registro único
```
SET <chave> <valor>
```
exemplo:
```
SET "resultado" 100
```

#### Recuperar registro único
```
GET <chave>
```
exemplo:
```
GET "resultado"
```

#### Verificar lista de registro Salvos
```
KEYS <pattners>
```
exemplo:
```
KEYS result*
```

#### Adicionar multiplos registro de uma única vez
```
SET <chave> <valor> <chave> <valor> <chave> <valor>
```
exemplo:
```
SET "resultado_1" 100 "resultado_2" 200 "resultado_3" 300
```
#### Adicionar e buscar e deletar HASH de registro
Para __adicionar__ vários registro seguimos o seguindo padrão: ```HSET <hash> <chave> <valor>```, o hash e a chave serão usadas para identificação do valor.

exemplo:
```
HSET "2022-07-12" "disciplina" "java"
HSET "2022-07-12" "horario" "14h"
HSET "2022-08-30" "disciplina" "react"
```

Para __buscar__ um hash usamos o  ```HGET```, ele retornará os valores de acordo com as informações do hash e da chave no seguinte padrão: ```HGET <hash> <chave>```
exemplo:
```
HGET "2022-07-12" "disciplina"
```
Resultado:
```
"2022-07-12" "disciplina" "java"
```
Para __Deletar__ utiliza-se o comando com a sintaxe: ```HDEL <hash> <chave>```  exemplo: ```HDEL "2022-08-30" "disciplina"``` 
Para trazer todos os dados de uma chave uliliza-se os __HGETALL__

#### Adicionar Multiplos Registros de uma só vez com HASH
para adicionar multiplos registros hash de uma só vez, utiliza-se a sintaxe: ```HMSET <hash> <chave> <valor> <chave> <valor> <chave> <valor>```

exemplo:
 ```
 HMSET <hash> <chave> <valor> <chave> <valor> <chave> <valor>
  HMSET "2022-07-01" "Nome" "Diego" "Sobrenome" "Couto" "github" "dccouto"
 ```
 
## Descobrindo o tipo da chave
Para descolbri o tipo da ```<chave>``` utliza-se o comando ```TYPE <chave>```
 
## Bitset 
#### Inserindo e lendo
+ O ```SETBIT``` permite que adicione informação booleanas a um registro, a sintaxe é ```SETBIT <hash> <chave_maior_que_0> <valor_booleano>```
exemplo:
```
SETBIT acesso:2022-07-12 10 1
```
+ O ```GETBIT``` permite que adicione informação booleanas a um registro, a sintaxe é ```SETBIT <hash> <chave_maior_que_0> <valor_booleano>```
exemplo:
```
GETBIT acesso:2022-07-12 10 1
```
Caso a chave não recebeu um valou 1 ela automaticamente será 0 quando realizar o ```GETBIT```

#### Contando os valores com bit
Para pegar o total de hashs que tem o bit setado com 1, utiliza-se o comando ```BITCOUNT <hash>```
exemplo:
```
BITCOUNT acesso:2022-07-12
```
## Operação bits
Para realizar operações com bits, utilizasse o comando BITOP

#### Operador AND
Para aplicar operações(BITOP) de '&' utiliza-se o AND, de acordo com a seguinte sintasse ````BITOP AND <nova_hash_do_resultado> <hash1_que_ira_aplicar_o_&> <hash2_que_ira_aplicar_o_&>```

exemplo:
```
//definindo acesso a sala 1 e 2
SETBIT acesso_sala:2022-07-12 1 1
SETBIT acesso_sala:2022-07-13 1 1
SETBIT acesso_sala:2022-07-13 2 1

//fazendo o AND do dia 12 e 13
BITOP AND acesso_sala:2022-07-12-e-13 acesso_sala:2022-07-12 acesso_sala:2022-07-13

//verificandoo reusltado da operação AND na qual irá mostrar qual a sala que teve acesso no dia 12 e 13
GETBIT acesso_sala:2022-07-12-e-13 1
(integer) 1 //Teve Acesso a sala nos 2 dias

GETBIT acesso_sala:2022-07-12-e-13 2
(integer) 0 //Não teve Acesso a sala nos 2 dias
```

#### Operador OR
Para aplicar operações(BITOP) de '||' utiliza-se o OR, de acordo com a seguinte sintasse ````BITOP OR <nova_hash_do_resultado> <hash1_que_ira_aplicar_o_OR> <hash2_que_ira_aplicar_o_OR>```

exemplo:
```
//definindo acesso a sala 1 e 2
SETBIT acesso_sala:2022-07-12 1 1
SETBIT acesso_sala:2022-07-13 1 1
SETBIT acesso_sala:2022-07-13 2 1

//fazendo o OR do dia 12 e 13
BITOP OR acesso_sala:2022-07-12-OU-13 acesso_sala:2022-07-12 acesso_sala:2022-07-13

//verificandoo reusltado da operação OR na qual irá mostrar qual a sala que teve acesso no dia 12 ou 13
GETBIT acesso_sala:2022-07-12-OU-13 1
(integer) 1 //Teve Acesso a sala no dia 12 ou no dia 13

GETBIT acesso_sala:2022-07-12-OU-13 2
(integer) 1 //Teve Acesso a sala no dia 12 ou no dia 13
```


## Expirar registros
Para expirar/deletar registros do banco do Redis, a sintaxe é ```EXPIRE <chave> <tempo_em_segundos>```
exemplo:
```EXPIRE "sessao:usuario:1" 1800```

Para checar quanto tempo falta pra expirar usar o comando ```TTL <chave>```
exemplo:
```
TTL "sessao:usuario:1"
```

## Buscas personalizadas
Buscar sorteios da Mega-Sena que ocorreram nos dias terminados em "3" ou em "7".
```
<ip> KEYS "resultado:?3-??-????:megasena"
1) "resultado:03-05-2015:megasena"

<ip>KEYS "resultado:?7-??-????:megasena"
1) "resultado:17-05-2015:megasena"
```
Mas queremos pesquisar os dois ao mesmo tempo, então usamos "[]" (colchetes):
```
<ip> KEYS "resultado:?[37]-??-????:megasena"
1) "resultado:03-05-2015:megasena"
2) "resultado:17-05-2015:megasena"
```
## Manipulando dados
#### Incremento e decremento de valor
+ Para incrementar em +1, utilize o seguinte padrão: ```INCR <chave>```. E para decrementar em -1 utilize o ```DECR <chave>```
+ Para incrementar em x valor __inteiro__ , utilize o seguinte padrão: ```INCRBY <chave> <valor>```. E para decrementar em x valor __inteiro__  utilize o 
```DECRBY <chave> <valor>```
+ Para incrementar em x valor __float__ , utilize o seguinte padrão: ```INCRBYFLOAT <chave> <valor>```. E para decrementar em x valor __float__  utilize o 
```DECRBYFLOAT <chave> <valor>```

## Listas, Ordenação e limites
### Adicionando elementos na lista com Limites
#### LPUSH para adicionar elementos na lista
Para exemplificar irei tratar como uma lista de ultimas nootícias, onde as 3 ultimas notícias serão mantidas no Redis na seguinte regra: AS notícias recem inseridas ficaram no top, e só serão mantidas 3 notícias na lista.
Para conseguir inserir uma notícia na lista e ela sendo a primeira, utiliza-se o comando ```LPUSH``` onde 'L' significa Left (primeira da lista) e 'PUSH' é o comando de inserir. 
A sintaxe é ```LPUSH <chave> <valor> ``` 

Exemplo:
```
LPUSH ultimas-noticias "Estudo de Redis na segunda-feira."
LPUSH ultimas-noticias "Estudo de Java na terça-feira."
LPUSH ultimas-noticias "Estudo de AWS na quarta-feira."
LPUSH ultimas-noticias "Estudo de Kafka na quinta-feira."
```
#### LTRIM para cortar elementos fora do range na lista

de acordo com o exemplo proposto, para manter as três últimas notícias na lista de noticícas, é necessário manter as notícias da posição 0 a posição 2. O comando para cortar tudo que não estiver no intervalo desejado é o ```LTRIM```. A sintexe é ```LTRIM <chave> <posição_inicial , posição_final>```
Exemplo:
```
LTRIM ultimas-noticias 0,2
```
#### LINDEX para buscar um elemento da Lista
Para pegar um elemento da lista pelo index, utiliza-se o ```LINDEX``` de acordo com a seguinte sintaxe: ```LINDEX <chave> <index>```

exemplo:
```
LINDEX ultimas-noticias 1
resultado: "Estudo de Java na terça-feira."
```
#### LLEN para saber o tamanho da Lista
Para pegar o length da lista, utiliza-se o ```LLEN```  de acordo com a seguinte sintaxe: ```LLEN <chave>```
exemplo:
```
LLEN ultimas-noticias
```

#### LRANGE para pelar um range da Lista
Para pegar o range de elementos da lista, utiliza-se o ```LRANGE``` de acordo com a seguinte sintaxe: ```LRANGE <chave> <posição_inicial , posição_final>```

exemplo:
```
LRANGE ultimas-noticias 1,2
Resultado:  "Estudo de Java na terça-feira."
            "Estudo de AWS na quarta-feira."
```

## Fila
Para exemplificar irei tratar como uma fila para atendimento que será mantidaa no Redis na seguinte regra: O primeiro a entrar na fila é o primeiro a ser atendido.
### Adicionando elementos na fila
#### RPUSH para adicionar elementos no fim da fila
Para adicionar um elemento no final da fila utiliza-se o comando ```RPUSH```, a sintaxe do comando é: ```RPUSH <chave> <valor>```

Exemplo:
```
RPUSH "fila:confirma-email" "pessoa_1_@mail.com"
RPUSH "fila:confirma-email" "pessoa_2_@mail.com"
RPUSH "fila:confirma-email" "pessoa_3_@mail.com"
RPUSH "fila:confirma-email" "pessoa_4_@mail.com"
```
### Consumindo elementos da fila
#### LPOP para pegar e retirar um elemento da fila
Para pegar e retitar o primeiro elemento da fila utiliza-se o ```LPOP```. A sintaxe é ```LPOP <chave>```
Exemplo:
```
LPOP "fila:confirma-email"
Resultado: "pessoa_1_@mail.com"
```

### Tratando o problema de Busy Wait (Espera ocupada)
Qual tenta buscar alguém na fila e não tem ninguém e repete esse processo diversas vezes com o ```LPOP "fila:confirma-email"``` ocorre o provlema de ocupar processamento com diversas checagem na fila sem ter ninguém, pra resolver esse problemas podemos fazer uma busca na fila estipulando o tempo de espera com o comando ```BLPOP```, com esse comando definimos um tempo em que ele irá aguardar algum dado chegar na fila e com isso não irá travar processamento. A sintaxe do comnado é: ```BLPOP <chave> <tempo_em_segundos> (se colocar 0 é eterno)```

Exemplo:
```
BLPOP "fila:confirma-email" 30
Resultado: "pessoa_5_@mail.com"
(12segundos_depois)
```

## Conjunto de elementos

#### Adicionando itens no conjunto
O conjunto não possui elementos repetidos, para adicionar um item em um conjunto, utiliza-se ```SADD``` (como se fosse um add no SetList). A sintaxe do comando é: ```SADD <chave> <valor>```
Exemplo:
```
SADD "conexões-do-linkedin:Diego" "Julia"
Resultado: (integer) 1
SADD "conexões-do-linkedin:Diego" "Ana"
Resultado: (integer) 2
```
#### Total de itens no conjunto
O comando para pegar o número de elementos que tem no conjunto é o ```SCARD```,(card de cardinalidade) a sintaxe é ```SCARD <chave>```

Exemplo:
```
SCARD "conexões-do-linkedin:Diego" 
Resultado: (integer) 2
```
#### Listar membros do conjunto
O comando para listar os membros do conjunto (não é ordenada) é o ```SMEMBERS```, a sintaxe é ```SMEMBERS <chave>```

Exemplo:
```
SMEMBERS "conexões-do-linkedin:Diego" 
Resultado: 1) Julia
           2) Ana
```

#### Verificar se elemento já é membros do conjunto
O comando para verificar se o valor já é um membro do conjunto é o ```SISMEMBER```, a sintaxe é ```SISMEMBER <chave> <valor>```

Exemplo:
```
SISMEMBER "conexões-do-linkedin:Diego" Karem
Resultado: 0
SISMEMBER "conexões-do-linkedin:Diego" Julia
Resultado: 1
```

#### Remover um membro do conjunto
O comando para remover um membro do conjunto é o ```SREM```, a sintaxe é ```SREM <chave> <valor>```

Exemplo:
```
SREM "conexões-do-linkedin:Diego" Karem
Resultado: 0
```

### Intersecção de conjunto
O comando para descobrir a intersecção(elementos que se __repetem__ nos 2 conjuntos) entre dois conjuntos é ```SINTER```, a sintaxe é ```SINTER <chave> <chave>```

Exemplo:
```
SINTER "conexões-do-linkedin:Diego" "conexões-do-linkedin:Juliana"
Resultado: 1) Clara
```

### Diferença entre conjunto
O comando para descobrir a diferença(elementos que __não__ se repetem nos 2 conjuntos) entre dois conjuntos é ```SDIFF```, a sintaxe é ```SDIFF <chave> <chave>```

Exemplo:
```

Fulano = (Zé, Beltrano, Deltrano)
Ciclano = (Zé)
SINTER "conexões-do-linkedin:Fulano" "conexões-do-linkedin:Ciclano"
Resultado: 1) Beltrano
           2) Deltrano
```

### União entre conjunto
O comando para descobrir a união entre dois conjuntos é ```SUNION ```, a sintaxe é ```SUNION  <chave> <chave>```

Exemplo:
```

Fulano = (Zé, Beltrano, Deltrano)
Ciclano = (Zé)
SUNION "conexões-do-linkedin:Fulano" "conexões-do-linkedin:Ciclano"
Resultado: 1) Beltrano
           2) Deltrano
           3) Zé
```
