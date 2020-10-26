# Documentação API - Contentores

Este é um objecto que representa um recipiente físico utilizado para a depisição de um tipo específico de resíduos, definido como [produto](../dopbase/produto.md).
É possível obter um conjunto de contentores ou a informação de cada contentor isoladamente. 

o endpoint só aceita os verbos GET, HEAD e OPTIONS e localiza-se em:

```http request
api/contentores/contentores
```


&nbsp;
## O objecto contentor

Cada objecto obtêm-se isoladamente através do seu id utilizando, por exemplo:
 
```http request 
GET api/contentores/contentores/4173
```
devolve o objecto com id igual a 4173, com a seguinte estrutura:

```json 
{
    "id": 4173,
    "idcontentor": "481824",
    "idtransponder": "962C391000004000",
    "produto": "ind",
    "tipo": "IND800",
    "capacidade": 800,
    "datainicio": "2018-05-06",
    "datafim": null,
    "circuitos": [
          "ind_180",
          "ind_120"
          ],
    "ativo": true,
    "uri": "https://cabi.pt/api/contentores/contentores/4173",
    "data_ultima_recolha": null,
    "data_ultima_lavagem": "2020-08-25",
    "niveis_dict": {
          "1": 0,
          "2": 0,
          "3": 0,
          "4": 0,
          "5": 0,
          "Sem nível": 0
     }
}
    
```

&nbsp;
### Atributos

Atríbuto | Tipo | Descrição | Opções
-------- | ---- | --------- | ------
id | inteiro | chave primaria | -  
idcontentor | string | código único do contentor no sistema MOBA | -  
idtransponder | string | código único do tag (associado ao contentor) no sistema MOBA | -  
produto | string | nome do produto do contentor | -  
tipo | string | categoria (produto + capacidade) | IND120<br/>IND240<br/>IND800<br/>IND3000<br/>PAP800<br/>PAP1100<br/>PAP2500<br/>PAP5000<br/>PLA800<br/>PLA1100<br/>PLA2500<br/>PLA5000<br/>VID2500<br/>VID3000<br/>ORG240<br/>ORG660<br/>ORG800
produto | inteiro | capacidade de deposição em litros | -  
circuitos | array de strings | lista com os nome dos circuitos que estão associados ao contentor |
ativo | boleano | estado | True<br />False
uri | string (link) | uniform resource identifier do recurso na API | 
geom | objecto geoespacial (ponto) | localização geoespacial (srid=4326) |
datainicio | data | data de inserção no sistema MOBA |
datafim | data | data de desativação |
data_ultima_recolha | data | data da recolha mais recente |
data_ultima_lavagem | data | data da lavagem mais recente |
niveis_dict | hash | contagem dos níveis de enchimento das recolhas do contentor. Apenas aplicável a contentores de papel, plástico e vidro |

&nbsp;
## Listas de contentores

As listagens de contentores podem ser obtidas no endpoint respectivo, é possível [filtar](#filtragem), [ordenar](#ordenação) e [procurar](#procurar) os resultados.
```http request
GET api/contentores/contentores/
```
A resposta venda seguinte forma:

```json
{
    "count": 12712,
    "next": "https://cabi.pt/api/contentores/contentores/?page=3",
    "previous": "https://cabi.pt/api/contentores/contentores/",
    "results": [ 
       "contentor1",
       "contentor2",
       "..."
       "contentorn"
       ],
}
```
Onde:
* `count` representa o total de objectos devolvidos
* `next` representa o link para a pŕoxima página
* `previous` representa o link para a página anterior
* `results` representa a lista de resultados até ao máximo do número de elementos por página


As respostas vêm paginadas com 20 objecto. É possível alterar o número de registos de cada página através do query parameter ‘per_page’ até ao máximo de 100000 registos.

```http request
GET api/contentores/contentores
```
devolve os resultado com 20 elementos por página
```http request
GET api/contentores/contentores/?per_page=1000
```
devolve os resultado com 1000 elementos por página


&nbsp;
## Filtragem

Os filtros deveram ser adicionados após o simbolo `?` Separando cada filtro por `&` por exemplo

```http request
GET api/contentores/contentores/?tipo=IND800&idcontentor__icontains=842
```

Podemos utilizar os seguinte critérios de filtragem:
* [tipo](#tipo)
* [circuito](#circuito)
* [idcontentor](#idcontentor)
* [idtransponder](#idtransponder)
* [local](#capacidade)
* [ativo](#ativo)
* [datainicio](#datainicio)
* [datafim](#datafim)
* [data_ultima_recolha](#data_ultima_recolha)
* [data_ultima_lavagem](#data_ultima_lavagem)

&nbsp;
### tipo

Filtrar por `tipo` devolve todos os resultados com tipo igual ao parâmetro.

Filtrar por `tipo__iexact` devolve todos os resultados com tipo igual ao parâmetro, de modo não case sensitive.

Filtrar por `tipo__icontains` devolve todos os resultados que o tipo contém o parâmetro, de modo não case sensitive.

```http request
GET api/contentores/contentores/?tipo=IND3000
```
&nbsp;
### circuito

Filtrar por `circuito` devolve todos os resultados que contém o parâmetro de pesquisa em qualquer elemento do array de circuitos.
A pesquisa não é case sensitive, exemplo:

```http request
GET  api/contentores/contentores/?circuitos=nd_11
```
&nbsp;
### idcontentor

Filtrar por `idcontentor` devolve todos os resultados com idcontentor igual ao parâmetro.

Filtrar por `idcontentor__iexact` devolve todos os resultados com idcontentor igual ao parâmetro, de modo não case sensitive.

Filtrar por `idcontentor__icontains` devolve todos os resultados que o idcontentor contém o parâmetro, de modo não case sensitive.

```http request
GET api/contentores/contentores/?idcontentor__icontains=2
```
&nbsp;
### idtransponder

Filtrar por `idtransponder` devolve todos os resultados com idtransponder igual ao parâmetro.

Filtrar por `idtransponder__iexact` devolve todos os resultados com idtransponder igual ao parâmetro, de modo não case sensitive.

Filtrar por `idtransponder__icontains` devolve todos os resultados que o idtransponder contém o parâmetro, de modo não case sensitive.

```http request
GET api/contentores/contentores/?idtransponder__iexact=002
```
&nbsp;
### capacidade

Filtrar por `capacidade` devolve todos os resultados com capacidade igual ao parâmetro.

Filtrar por `capacidade__lte` devolve todos os resultados em que a capacidade é menor ou igual ao parâmetro de pesquisa (data)

Filtrar por `capacidade__lt` devolve todos os resultados em que a capacidade é menor do que o parâmetro de pesquisa (data)

Filtrar por `capacidade__gte` devolve todos os resultados em que a capacidade é maior ou igual ao parâmetro de pesquisa (data)

Filtrar por `capacidade__gt` devolve todos os resultados em que a capacidade é maior do que o parâmetro de pesquisa (data)

```http request
GET api/contentores/contentores/?capacidade=800
```
&nbsp;
### ativo

Filtrar por `esta_ativo` devolve todos os resultados em que o ativo é igual ao parâmetro, exemplo:

```http request
GET api/contentores/contentores/?esta_ativo=true
```
&nbsp;
### datainicio

Filtrar por `datainicio__lte` devolve todos os resultados em que a datainicio é menor ou igual ao parâmetro de pesquisa (data)

Filtrar por `datainicio__lt` devolve todos os resultados em que a datainicio é menor do que o parâmetro de pesquisa (data)

Filtrar por `datainicio__gte` devolve todos os resultados em que a datainicio é maior ou igual ao parâmetro de pesquisa (data)

Filtrar por `datainicio__gt` devolve todos os resultados em que a datainicio é maior do que o parâmetro de pesquisa (data)

Filtrar por `datainicio__dias` devolve todos os resultados em que a datainicio ocorreu nos últimos `n` dias, em que `n` é o parâmetro de pesquisa (número)

Exemplos:

```http request
GET api/contentores/contentores/?datainicio__dias=50

GET api/contentores/contentores/?datainicio__gte=2020-01-01
```
&nbsp;
### datafim

Filtrar por `datafim__lte` devolve todos os resultados em que a datafim é menor ou igual ao parâmetro de pesquisa (data)

Filtrar por `datafim__lt` devolve todos os resultados em que a datafim é menor do que o parâmetro de pesquisa (data)

Filtrar por `datafim__gte` devolve todos os resultados em que a datafim é maior ou igual ao parâmetro de pesquisa (data)

Filtrar por `datafim__gt` devolve todos os resultados em que a datafim é maior do que o parâmetro de pesquisa (data)

Filtrar por `datafim__dias` devolve todos os resultados em que a datafim ocorreu nos últimos `n` dias, em que `n` é o parâmetro de pesquisa (número)

Exemplos:

```http request
GET api/contentores/contentores/?datafim__dias=50

GET api/contentores/contentores/?datafim__gte=2020-01-01
```
&nbsp;
### data_ultima_recolha

Filtrar por `data_ultima_recolha__lte` devolve todos os resultados em que a data_ultima_recolha é menor ou igual ao parâmetro de pesquisa (data)

Filtrar por `data_ultima_recolha__lt` devolve todos os resultados em que a data_ultima_recolha é menor do que o parâmetro de pesquisa (data)

Filtrar por `data_ultima_recolha__gte` devolve todos os resultados em que a data_ultima_recolha é maior ou igual ao parâmetro de pesquisa (data)

Filtrar por `data_ultima_recolha__gt` devolve todos os resultados em que a data_ultima_recolha é maior do que o parâmetro de pesquisa (data)

Filtrar por `data_ultima_recolha__dias` devolve todos os resultados em que a data_ultima_recolha ocorreu nos últimos `n` dias, em que `n` é o parâmetro de pesquisa (número)

Exemplos:

```http request
GET api/contentores/contentores/?data_ultima_recolha__dias=50

GET api/contentores/contentores/?data_ultima_recolha__gte=2020-01-01
```
&nbsp;
### data_ultima_lavagem

Filtrar por `data_ultima_lavagem__lte` devolve todos os resultados em que a data_ultima_lavagem é menor ou igual ao parâmetro de pesquisa (data)

Filtrar por `data_ultima_lavagem__lt` devolve todos os resultados em que a data_ultima_lavagem é menor do que o parâmetro de pesquisa (data)

Filtrar por `data_ultima_lavagem__gte` devolve todos os resultados em que a data_ultima_lavagem é maior ou igual ao parâmetro de pesquisa (data)

Filtrar por `data_ultima_lavagem__gt` devolve todos os resultados em que a data_ultima_lavagem é maior do que o parâmetro de pesquisa (data)

Filtrar por `data_ultima_lavagem__dias` devolve todos os resultados em que a data_ultima_lavagem ocorreu nos últimos `n` dias, em que `n` é o parâmetro de pesquisa (número)

Exemplos:

```http request
GET api/contentores/contentores/?data_ultima_lavagem__dias=50

GET api/contentores/contentores/?data_ultima_lavagem__gte=2020-01-01
```


&nbsp;
## Ordenação

Os resultados podem ser ordenados utilizando os seguintes atributos:
```http request
idcontentor
idtransponder
produto
capacidade
```

Para se ordenar utiliza-se o parâmetro `ordering` que pode conter um número indiscriminado de argumentos separados por vírgulas.

Se se usar o nome do atriuto a ordem é crescente, caso o nome seja precedido por `-` a ordem é descrescente.

Por exemplo:


```http request
GET api/contentores/contentores/?ordering=datafim,-idcontentor,-capacidade
```
Ordena os resultados primeiro por `datafim` (ordem crecente), depois por `idcontentor` (ordem decrescente) e finalmente por `capacidade` (ordem decrescente)


&nbsp;
## Procura

podem efectuar-se procurar nos seguintes atributos:
```http request
idcontentor
idtransponder
```

Para se ordenar utiliza-se o parâmetro `q`, por exemplo:


```http request
GET api/contentores/contentores/?q=as
```
devolve os resultados onde "as" faz parte de um, ou mais, dos atributos de pesquisa 

