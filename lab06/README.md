# Modelo para Apresentação do Lab06 - Patologias, Medicamentos e Efeitos Colaterais em Cypher

Estrutura de pastas:

~~~
├── README.md  <- arquivo apresentando a tarefa
~~~

# Equipe `<nome da equipe>`

# Subgrupo `<letra do subgrupo>`
* `Davi Gabriel Bandeira Coutinho` - `183710`
* `Francisco Vinicius Sousa Guedes` - `260440`
* `<nome completo>` - `<RA>`

## Tarefa de Cypher sobre Patologias, Medicamentos e Efeitos Colaterais

## Exercício

Faça a projeção em relação a Patologia, ou seja, conecte patologias que são tratadas pela mesma droga.

### Resolução
Carregando as patologias
~~~cypher
LOAD CSV WITH HEADERS FROM 'https://raw.githubusercontent.com/santanche/lab2learn/master/data/faers-2017/drug.csv' AS line
CREATE (:Drug {code: line.code, name: line.name})
~~~
Criando índice
~~~cypher
CREATE INDEX FOR (p:Pathology) ON (p.code)
~~~
Carregando os remédios
~~~cypher
LOAD CSV WITH HEADERS FROM 'https://raw.githubusercontent.com/santanche/lab2learn/master/data/faers-2017/pathology.csv' AS line
CREATE (:Pathology { code: line.code, name: line.name})
~~~
~~~cypher
CREATE INDEX FOR (p:Pathology) ON (p.code)
~~~
Criando a relação de 'Droga trata Patologia'
~~~cypher
LOAD CSV WITH HEADERS FROM 'https://raw.githubusercontent.com/santanche/lab2learn/master/data/faers-2017/drug-use.csv' AS line
MATCH (d:Drug {code: line.codedrug})
MATCH (p:Pathology {code: line.codepathology})
MERGE (d)-[t:Treats]->(p)
ON CREATE SET t.weight=1
ON MATCH SET t.weight=t.weight+1
~~~
Relação entre as patologias que são tratadas pela mesma droga.
~~~cypher
MATCH (d1:Pathology)<-[a]-(p:Drug)-[b]->(d2:Pathology)
WHERE a.weight > 20 AND b.weight > 20 and d1.name <> d2.name
MERGE (d1)<-[r:Relates]->(d2)
ON CREATE SET r.weight=1
ON MATCH SET r.weight=r.weight+1
~~~



# Trabalhando com Efeitos Colaterais

## Exercício

Construa um grafo ligando os medicamentos aos efeitos colaterais (com pesos associados) a partir dos registros das pessoas, ou seja, se uma pessoa usa um medicamento e ela teve um efeito colateral, o medicamento deve ser ligado ao efeito colateral.

### Resolução
Utilizando os nós que foram carregados no exercício anteiror
Carregando os índices das pessoas
~~~cypher
LOAD CSV WITH HEADERS FROM 'https://raw.githubusercontent.com/santanche/lab2learn/master/data/faers-2017/drug-use.csv' AS line
CREATE (:Person{code: line.idperson})
~~~
Criando índices para os nós de Pessoas
~~~cypher
CREATE INDEX FOR (p:Person) ON (p.code)
~~~
Relacionando Drogas e pessoas
~~~cypher
CREATE INDEX FOR (p:Person) ON (p.code)
~~~
Relacionando as drogas e as pessoas
~~~cypher
LOAD CSV WITH HEADERS FROM 'https://raw.githubusercontent.com/santanche/lab2learn/master/data/faers-2017/drug-use.csv' AS line
MATCH (d:Drug {code: line.codedrug})
MATCH (p:Person {code: line.idperson})
MERGE (p)-[t:Use]->(d)
ON CREATE SET t.weight=1
ON MATCH SET t.weight=t.weight+1
~~~
Relacionando Pessoas e efeitos colaterais
~~~cypher
LOAD CSV WITH HEADERS FROM 'https://raw.githubusercontent.com/santanche/lab2learn/master/data/faers-2017/sideeffect.csv' AS line
MATCH (d:Pathology {code: line.codePathology})
MATCH (p:Person {code: line.idPerson})
MERGE (p)-[t:HaveSideEffect]->(d)
ON CREATE SET t.weight=1
ON MATCH SET t.weight=t.weight+1
~~~
Relacinando Efeitos Colaterais e drogas pelo intermediário
~~~cypher
MATCH (d1:Drug)<-[a]-(p:Person)-[b]->(d2:Pathology)
WHERE d1.name <> d2.name
MERGE (d1)-[r:Cause]->(d2)
ON CREATE SET r.weight=1
ON MATCH SET r.weight=r.weight+1
~~~
## Exercício

Que tipo de análise interessante pode ser feita com esse grafo?

Proponha um tipo de análise e escreva uma sentença em Cypher que realize a análise.

### Resolução
~~~cypher
(escreva aqui a resolução em Cypher)
~~~
