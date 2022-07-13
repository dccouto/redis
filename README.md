# Banco de Dados Redis

### Instalação e Utilização
instalar o Redis no docker com o comando ```docker pull redis``` , e depois de executar utilizar o comando ```redis-cli```
### Comandos

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

#### Adicionar Multiplos Registros de uma só vez com HASH
para adicionar multiplos registros hash de uma só vez, utiliza-se a sintaxe: ```HMSET <hash> <chave> <valor> <chave> <valor> <chave> <valor>```

exemplo:
 ```
 HMSET <hash> <chave> <valor> <chave> <valor> <chave> <valor>
  HMSET "2022-07-01" "Nome" "Diego" "Sobrenome" "Couto" "github" "dccouto"
 ```
#### Expirar registros
Para expirar/deletar registros do banco do Redis, a sintaxe é ```EXPIRE <chave> <tempo_em_segundos>```
exemplo:
```EXPIRE "sessao:usuario:1" 1800```

Para checar quanto tempo falta pra expirar usar o comando ```TTL <chave>```
exemplo:
```
TTL "sessao:usuario:1"
```

#### Buscas personalizadas
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
