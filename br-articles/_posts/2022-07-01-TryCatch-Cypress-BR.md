---
layout: post
title:  "Implementando Try / Catch no Cypress de Maneira Recursiva"
date:   2022-07-01-0100
category: BR-Articles
---
<!-- © 2022 Bruno Longo <bruno_pereira_longo@hotmail.com> -->
<p+> Olá, bem vindo(a). Nesse artigo vamos abordar a ideia de como e porque implementar um Try/Catch no Cypress de modo recursivo.</p>

<p> Esse exemplo busca mostrar as facilidades e benefícios de se usar o Try/Catch em requisição a API's, porem, com base nele você pode conseguir implementar o Try/Catch em diversas outras instâncias.</p>

***ATENÇÃO: Não irei cobrir o básico sobre Cypress nesse artigo então é importante que se tenha uma leve noção de Cypressa além de Programação Orientada a Objeto.***

***ATENÇÃO-2: Vamos utilizar as API's da <a href="https://api.nasa.gov/" target="_blank" >NASA</a> para todos os testes usando um token basico porem o site fornece um proprio que da acesso a mais requisição por dia.***

<p>Vale também salientar que o exemplo aqui abordado é de livre uso, ou seja, Open Source fique a vontade para copiar e usar em seus projeto, porem, para publicação de artigos, usa acadêmico e outros similares peço apenas que me coloque como referencia e se possível me marque na publicação, ficarei muito feliz com a divulgação do meu trabalho.</p>

## A Variavel Request

Para utilizarmos o try/catch da melhor forma possível primeiro precisamos criar uma variável para ser nossa requisição a API. Veja que podemos criar uma request como variavel fora do custom command ou como parte de um custom command, nesse exemplo vamos usar dentro do custom command.

```javascript
Cypress.Commands.add('Get Astronomy Picture of the Day', function(){
    const request = { 
        "method": 'GET', 
        "url": 'https://api.nasa.gov/planetary/apod',
        "headers": { 
            'Content-Type': 'application/json', 
            'Authorization': 'Bearer '+ testObject.glToken, 
            'Accept': 'application/json' 
        } 
    };
```

```javascript 
    let retries = -1; 
    function makeRequest() {
        retries++ 
        cy.log("Attempt Number " + retries)
        cy.request(request)
        .then(function (response) {
            try { 
                expect(response.status).to.be.equal(200) 
            }
            catch (err) {
                if (retries > 59) throw new Error(`Retried too many times (${--retries})`)
                cy.log(response.body)
                cy.log(err)
                cy.wait(Cypress.env('timeSuperFast')) 
                return makeRequest();
            }
        })
        .should(function (response) { if (response.body != null) { 
            
        })
    }
    return makeRequest()
});
```
