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
Para adicionar vários registro seguimos o seguindo padrão: ```HSET <chave1> <chave2> <valor>```, a chave 1 e 2 serão usadas como hash.

exemplo:
```
HSET "2022-07-12" "disciplina" "java"
HSET "2022-07-12" "horario" "14h"
HSET "2022-08-30" "disciplina" "react"
```

Para buscar um hash usamos o  ```HGET```, ele retornará os valores de acordo com as informações da chave 1 e 2 no seguinte padrão: ```HGET <chave1> <chave2>```
exemplo:
```
HGET "2022-07-12" "disciplina"
```
Resultado:
```
"2022-07-12" "disciplina" "java"
```
Para Deletar utiliza-se o comando com a sintaxe: ```HDEL <chave1> <chave2>```  exemplo: ```HDEL "2022-08-30" "disciplina"``` 

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
