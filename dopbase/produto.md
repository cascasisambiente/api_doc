# Documentação API - Produto

Este é um objecto que um tipo específico de resíduos.
É possível obter um conjunto de locais ou a informação de cada local isoladamente. 

o endpoint só aceita os verbos GET, HEAD e OPTIONS e localiza-se em:

```http request
api/dopbase/produtos
```


&nbsp;
## O objecto produto

Cada objecto obtêm-se isoladamente através do seu id utilizando, por exemplo:
 
```http request 
GET api/dopbase/produtos/3/
```
devolve o objecto com id igual a 3, com a seguinte estrutura:

```json 
{
    "produto_id": 3,
    "produto": "Papel Cartão",
    "sigla": "PAP",
    "servico": {
        "servico_id": 2,
        "servico": "Selectiva",
        "sigla": "SLT"
    }
}
    
```

&nbsp;
### Atributos

Atríbuto | Tipo | Descrição | Opções
-------- | ---- | --------- | ------
produto_id | inteiro | chave primaria | -  
produto | string | nome do produto de resíduos conforme definição da TRATOLIXO | -  
sigla | string | sigla da produto | -  
[servico](servico.md) | hash | campos do [serviço](servico.md) associado ao produto | -  

&nbsp;
## Listas de produtos

As listagens podem ser obtidas no endpoint respectivo.

```http request
GET api/dopbase/produtos
```
A resposta venda na forma de array de produtos:

```json
[
    {
        "produto_id": 12,
        "produto": "Embalagens de Plástico (EPS-Esferovite)",
        "sigla": "Esferovite",
        "servico": {
            "servico_id": 5,
            "servico": "Outros",
            "sigla": "OUT"
        }
    },
    {
        "produto_id": 18,
        "produto": "Embalagens de Plástico PEAD (Bidons)",
        "sigla": "Bidons",
        "servico": {
            "servico_id": 5,
            "servico": "Outros",
            "sigla": "OUT"
        }
    },
    ....
]
```

