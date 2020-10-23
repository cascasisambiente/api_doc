# Local

Este é um objecto que representa um local físico que agrupa um conjunto de [contentores](#contentor) utilizados para a deposição de resíduos.
É possível obter um conjunto de locais ou a informação de cada local isoladamente. 

o endpoint só aceita os verbos GET, HEAD e OPTIONS e localiza-se em:

```http request
api/contentores/locais
```

&nbsp;
## O objecto local

Cada objecto obtêm-se isoladamente através do seu id utilizando, por exemplo:
 
```http request 
GET api/contentores/locais/2517
```
devolve o objecto com id igual a 2517, com a seguinte estrutura:

```json 
{
    "id": 2517,
    "tipo": "Indiferenciado",
    "circuitos": [
        "ind_180",
        "ind_120"
        ],
    "codigolocal": "002633",
    "local": "N247",
    "localidade": "Alcabideche",
    "rua": "Rua do Cabo",
    "numero": "0",
    "ativo": true,
    "geom": {
        "type": "Point",
            "coordinates": [
                -9.471229553,
                38.764690399
            ]
        },
    "datainicio": "2018-05-06",
    "datafim": null,
    "elementos": [
        "Cais",
        "Fixador",
        "IND800"
        ],
    "data_ultima_recolha": null,
    "data_ultima_lavagem": "2020-08-25",
    "niveis_dict": {
        "1": 0,
        "2": 0,
        "3": 0,
        "4": 0,
        "5": 0,
        "Sem nível": 0
        },
    "contentores": [
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
        ]
    }
```

&nbsp;
### Atributos

Atríbuto | Tipo | Descrição | Opções
-------- | ---- | --------- | ------
id | inteiro | chave primaria | 
tipo | string | categoria | Domicílios<br />Stock<br />Comercial<br />Industrial<br />Aterro<br />Grande produtor<br />Seletiva<br />Misto<br />Indiferenciado<br />Usuário doméstico 
circuitos | array de strings | lista com os nome dos circuitos que estão associados ao local |
codigolocal | string | código único de identificação do local no sistema MOBA |
local | string | parte da morada conforme definido no sistema MOBA |
localidade | string | parte da morada conforme definido no sistema MOBA |
rua | string | parte da morada conforme definido no sistema MOBA |
numero | string | parte da morada conforme definido no sistema MOBA |
ativo | boleano | estado | true<br />false
geom | objecto geoespacial (ponto) | localização geoespacial (srid=4326) |
datainicio | data | data de inserção no sistema MOBA |
datafim | data | data de desativação |
data_ultima_recolha | data | data da recolha mais recente |
data_ultima_lavagem | data | data da lavagem mais recente |
elementos | array de strings | lista com o tipologia de cada elemento pertencente ao local |
niveis_dict | hash | contagem dos níveis de enchimento das recolhas no local. Apenas aplicável a contentores de papel, plástico e vidro |
contentores | array de hashes | lista com objectos do tipo [contentor](#contentor) |    


&nbsp;
## Listas de Locais

As listagens de locais podem ser obtidas no endpoint respectivo, é possível [filtar](#filtragem) e [ordenar](#ordenação) os resultados.

```http request
GET api/contentores/locais/
```
A resposta venda seguinte forma:

```json
{
    "count": 374,
    "next": "https://cabi.pt/apiv2/contentores/locais2/?page=7&tipo=sele",
    "previous": "https://cabi.pt/apiv2/contentores/locais2/?page=9&tipo=sele",
    "results": [ 
       "local1",
       "local2",
       "..."
       "localn"
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
GET api/contentores/locais
```
devolve os resultado com 20 elementos por página
```http request
GET api/contentores/locais/?per_page=1000
```
devolve os resultado com 1000 elementos por página



&nbsp;
### Filtragem

Os filtros deveram ser adicionados após o simbolo `?` Separando cada filtro por `&` por exemplo

```http request
GET api/contentores/locais/?tipo=indiferenciado&local=alcabideche
```

Podemos utilizar os seguinte critérios de filtragem:
* [tipo](#tipo)
* [circuito](#circuito)
* [codigolocal](#codigolocal)
* [local](#local)
* [localidade](#localidade)
* [rua](#rua)
* [numero](#numero)
* [elementos](#elementos)
* [ativo](#ativo)
* [contagem de contentores](#contagem-de-contentores)
* [datainicio](#datainicio)
* [datafim](#datafim)
* [data_ultima_recolha](#data_ultima_recolha)
* [data_ultima_lavagem](#data_ultima_lavagem)

&nbsp;
##### tipo

Filtrar por `tipo` devolve todos os resultados que contém o parâmetro de pesquisa no atríbuto tipo.
A pesquisa não é case sensitive, exemplo:

```http request
GET api/contentores/locais/?tipo=indifer
```
&nbsp;
##### circuito

Filtrar por `circuito` devolve todos os resultados que contém o parâmetro de pesquisa em qualquer elemento do array de circuitos.
A pesquisa não é case sensitive, exemplo:

```http request
GET  api/contentores/locais/?circuitos=nd_11
```
&nbsp;
##### codigolocal

Filtrar por `codigolocal` devolve todos os resultados com codigolocal igual ao parâmetro.

Filtrar por `codigolocal__iexact` devolve todos os resultados com codigolocal igual ao parâmetro, de modo não case sensitive.

Filtrar por `codigolocal__icontains` devolve todos os resultados que o codigolocal contém o parâmetro, de modo não case sensitive.

```http request
GET api/contentores/locais/?codigolocal__icontains=002
```
&nbsp;
##### local

Filtrar por `local` devolve todos os resultados com local igual ao parâmetro.

Filtrar por `local__iexact` devolve todos os resultados com local igual ao parâmetro, de modo não case sensitive.

Filtrar por `local__icontains` devolve todos os resultados que o local contém o parâmetro, de modo não case sensitive.

```http request
GET api/contentores/locais/?local__iexact=002
```
&nbsp;
##### localidade

Filtrar por `localidade` devolve todos os resultados com localidade igual ao parâmetro.

Filtrar por `localidade__iexact` devolve todos os resultados com localidade igual ao parâmetro, de modo não case sensitive.

Filtrar por `localidade__icontains` devolve todos os resultados que a localidade contém o parâmetro, de modo não case sensitive.

```http request
GET api/contentores/locais/?localidade=002
```
&nbsp;
##### rua

Filtrar por `rua` devolve todos os resultados com rua igual ao parâmetro.

Filtrar por `rua__iexact` devolve todos os resultados com rua igual ao parâmetro, de modo não case sensitive.

Filtrar por `rua__icontains` devolve todos os resultados que a rua contém o parâmetro, de modo não case sensitive.

```http request
GET api/contentores/locais/?rua__icontains=abril
```
&nbsp;
##### numero

Filtrar por `numero` devolve todos os resultados com numero igual ao parâmetro.

Filtrar por `numero__iexact` devolve todos os resultados com numero igual ao parâmetro, de modo não case sensitive.

Filtrar por `numero__icontains` devolve todos os resultados que o numero contém o parâmetro, de modo não case sensitive.

```http request
GET api/contentores/locais/?numero=7054
```
&nbsp;
##### elementos

Filtrar por `elementos` devolve todos os resultados que contém o parâmetro de pesquisa em qualquer elemento do array de elementos.
A pesquisa não é case sensitive, exemplo:

```http request
GET api/contentores/locais/?elementos=cais
```
&nbsp;
##### ativo

Filtrar por `esta_ativo` devolve todos os resultados em que o ativo é igual ao parâmetro, exemplo:

```http request
GET api/contentores/locais/?esta_ativo=true
```
&nbsp;
##### contagem de contentores

Filtrar por `contentores_count` devolve todos os resultados em que o número de contentores é igual ao parâmetro de pesquisa.

Filtrar por `contentores_count__lte` devolve todos os resultados em que o número de contentores é menor ou igual ao parâmetro de pesquisa.

Filtrar por `contentores_count__gte` devolve todos os resultados em que o número de contentores é maior ou igual ao parâmetro de pesquisa.

```http request
GET api/contentores/locais/?contentores_count__gte=50
```
&nbsp;
#### datainicio

Filtrar por `datainicio__lte` devolve todos os resultados em que a datainicio é menor ou igual ao parâmetro de pesquisa (data)

Filtrar por `datainicio__lt` devolve todos os resultados em que a datainicio é menor do que o parâmetro de pesquisa (data)

Filtrar por `datainicio__gte` devolve todos os resultados em que a datainicio é maior ou igual ao parâmetro de pesquisa (data)

Filtrar por `datainicio__gt` devolve todos os resultados em que a datainicio é maior do que o parâmetro de pesquisa (data)

Filtrar por `datainicio__dias` devolve todos os resultados em que a datainicio ocorreu nos últimos `n` dias, em que `n` é o parâmetro de pesquisa (número)

Exemplos:

```http request
GET api/contentores/locais/?datainicio__dias=50

GET api/contentores/locais/?datainicio__gte=2020-01-01
```
&nbsp;
#### datafim

Filtrar por `datafim__lte` devolve todos os resultados em que a datafim é menor ou igual ao parâmetro de pesquisa (data)

Filtrar por `datafim__lt` devolve todos os resultados em que a datafim é menor do que o parâmetro de pesquisa (data)

Filtrar por `datafim__gte` devolve todos os resultados em que a datafim é maior ou igual ao parâmetro de pesquisa (data)

Filtrar por `datafim__gt` devolve todos os resultados em que a datafim é maior do que o parâmetro de pesquisa (data)

Filtrar por `datafim__dias` devolve todos os resultados em que a datafim ocorreu nos últimos `n` dias, em que `n` é o parâmetro de pesquisa (número)

Exemplos:

```http request
GET api/contentores/locais/?datafim__dias=50

GET api/contentores/locais/?datafim__gte=2020-01-01
```
&nbsp;
#### data_ultima_recolha

Filtrar por `data_ultima_recolha__lte` devolve todos os resultados em que a data_ultima_recolha é menor ou igual ao parâmetro de pesquisa (data)

Filtrar por `data_ultima_recolha__lt` devolve todos os resultados em que a data_ultima_recolha é menor do que o parâmetro de pesquisa (data)

Filtrar por `data_ultima_recolha__gte` devolve todos os resultados em que a data_ultima_recolha é maior ou igual ao parâmetro de pesquisa (data)

Filtrar por `data_ultima_recolha__gt` devolve todos os resultados em que a data_ultima_recolha é maior do que o parâmetro de pesquisa (data)

Filtrar por `data_ultima_recolha__dias` devolve todos os resultados em que a data_ultima_recolha ocorreu nos últimos `n` dias, em que `n` é o parâmetro de pesquisa (número)

Exemplos:

```http request
GET api/contentores/locais/?data_ultima_recolha__dias=50

GET api/contentores/locais/?data_ultima_recolha__gte=2020-01-01
```
&nbsp;
#### data_ultima_lavagem

Filtrar por `data_ultima_lavagem__lte` devolve todos os resultados em que a data_ultima_lavagem é menor ou igual ao parâmetro de pesquisa (data)

Filtrar por `data_ultima_lavagem__lt` devolve todos os resultados em que a data_ultima_lavagem é menor do que o parâmetro de pesquisa (data)

Filtrar por `data_ultima_lavagem__gte` devolve todos os resultados em que a data_ultima_lavagem é maior ou igual ao parâmetro de pesquisa (data)

Filtrar por `data_ultima_lavagem__gt` devolve todos os resultados em que a data_ultima_lavagem é maior do que o parâmetro de pesquisa (data)

Filtrar por `data_ultima_lavagem__dias` devolve todos os resultados em que a data_ultima_lavagem ocorreu nos últimos `n` dias, em que `n` é o parâmetro de pesquisa (número)

Exemplos:

```http request
GET api/contentores/locais/?data_ultima_lavagem__dias=50

GET api/contentores/locais/?data_ultima_lavagem__gte=2020-01-01
```


&nbsp;
### Ordenação

Os resultados podem ser ordenados utilizando os seguintes atributos:
```http request
codigolocal
local
localidade
rua
numero
esta_ativo
datainicio
datafim
data_ultima_recolha
data_ultima_lavagem
tipo
```

Para se ordenar utiliza-se o parâmetro `ordering` que pode conter um número indiscriminado de argumentos separados por vírgulas.

Se se usar o nome do atriuto a ordem é crescente, caso o nome seja precedido por `-` a ordem é descrescente.

Por exemplo:


```http request
GET api/contentores/locais/?ordering=datafim,-rua,-local
```
Ordena os resultados primeiro por `datafim` (ordem crecente), depois por `rua` (ordem decrescente) e finalmente por `local` (ordem decrescente)


&nbsp;
&nbsp;
# Contentor


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


