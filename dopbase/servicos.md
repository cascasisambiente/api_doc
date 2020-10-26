# Documentação API - Serviço

Este é um objecto que define uma tipologia organizacional de resíduos.

O endpoint só aceita os verbos GET, HEAD e OPTIONS e localiza-se em:

```http request
api/dopbase/servicos
```


&nbsp;
## O objecto servico

Cada objecto obtêm-se isoladamente através do seu id utilizando, por exemplo:
 
```http request 
GET api/dopbase/servicos/3/
```
devolve o objecto com id igual a 3, com a seguinte estrutura:

```json 
{
    "servico_id": 3,
    "servico": "Verdes",
    "sigla": "CJV"
}
    
```

&nbsp;
### Atributos

Atríbuto | Tipo | Descrição | Opções
-------- | ---- | --------- | ------
servico_id | inteiro | chave primaria | -  
servico | string | nome do serviço | -  
sigla | string | sigla do serviço | -   

&nbsp;
## Listas de serviços

As listagens de servicos podem ser obtidas no endpoint respectivo.

```http request
GET api/dopbase/servicos
```
A resposta venda na forma de array de serviços:

```json
[
    {
        "servico_id": 1,
        "servico": "Indiferenciados",
        "sigla": "IND"
    },
    {
        "servico_id": 6,
        "servico": "Lavagem",
        "sigla": "MAL"
    },
    {
        "servico_id": 4,
        "servico": "Monstros",
        "sigla": "OFU"
    },
    ....
]
```
