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

