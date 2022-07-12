# Banco de Dados Redis
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
