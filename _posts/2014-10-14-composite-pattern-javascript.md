---
layout: post
title: Explorando o Composite pattern JavaScript
---

Design Patterns é um assunto bem comum em todas as linguagens de programação, é o mais importante assunto quando se trata em manutenibilidade de código. Um pattern é uma solução reutilizável que pode ser aplicada em um projeto desenvolvimento de software e, hoje vamos explorar o **Composite** pattern com implementação JavaScript.

<!--more-->

Composite pattern diz que um grupo de objetos pode ser tratado da mesma maneira que um objeto individual desse grupo.

Um uso bem comum de Composite Pattern que você provavelmente já tenha visto é o sistema de diretórios de um Sistema Operacional, que utiliza estrutura de dados em árvore (considere que temos uma pasta, que dentro dessa pasta temos várias outras pastas, e que dentro de cada uma dessas outras pastas temos mais algumas pastas, e assim por diante).

Nesse modelo, temos “N” objetos que possui "N" filhos que também pode ter mais "N" filhos, e etc. Mas a quantidade de objetos não importa, porque a implementação é a mesma para todos eles.

Ok, vamos ver isso melhor na prática.

### Exemplo: Construindo uma cidade com Composite Pattern

Vamos colocar a mão na massa e criar nosso proprio exemplo modificando um pouco o modelo apresentado à cima mas ainda seguindo a estrutura em árvore.

No nosso exemplo vamos construir duas cidades, que possuem bairros e que por sua vez possuem algumas casas (lembra da estrutura em árvore). Algo bem simples e que vai ficar parecido como isso:

```
| cities
|
|-- São Paulo/
|   |-- Liberdade
|   	|-- Casa 1
|   	|-- Casa 2
|   |-- Ipiranga
|   	|-- Casa 3
|
|-- Rio de Janeiro/
|   |-- Leblon
|   	|-- Casa 4
|   |-- Lapa
|   	|-- Casa 5
|   	|-- Casa 6
|
```

O objeto irá possuir os seguintes métodos específicos do nosso composite:

* **add**: adiciona um novo filho para o objeto;
* **remove**: remove um determinado filho do objeto;
* **getChild**: retorna um filho do objeto;

Método auxiliar:

* **getElement**: retorna o elemento HTML do objeto específico;


####Criamos o objeto "Cidade" que terá o formato composite
```javascript 
var City = function (title, id, className) {
    this.children = [];
    this.element = document.createElement('div');
    var h2 = document.createElement('h2');

    this.element.id = id;
    if (className) this.element.className = className;

    h2.textContent = title;
    this.element.appendChild(h2);

}

City.prototype = {
    add: function (child) {
        this.children.push(child);
        this.element.appendChild(child.getElement());
    },

    remove: function (child) {    
        for (var node, i = 0; node = this.getChild(i); i++) {
            if (node == child) {
                this.children.splice(i, 1);
                this.element.removeChild(child.getElement());
                return true;
            }
        }

        return false;
    },

    getChild: function (i) {
        return this.children[i];
    },
     
    getElement: function () {
        return this.element;
    }
}
```

####Instanciamos os objetos e montamos a estrutura
```javascript 
var cities = new City('', 'cities');
var saoPaulo = new City('São Paulo', 'sao-paulo');
var rioDeJaneiro = new City('Rio de Janeiro', 'rio-de-janeiro');
var liberdade = new City('Liberdade', 'liberdade');
var ipiranga = new City('Ipiranga', 'ipiranga');
var lapa = new City('Lapa', 'lapa');
var leblon = new City('Leblon', 'leblon');
var casa1 = new City('Casa 1', 'casa-1', 'composite-house');
var casa2 = new City('Casa 2', 'casa-2', 'composite-house');
var casa3 = new City('Casa 3', 'casa-3', 'composite-house');
var casa4 = new City('Casa 4', 'casa-4', 'composite-house');
var casa5 = new City('Casa 5', 'casa-5', 'composite-house');
var casa6 = new City('Casa 6', 'casa-6', 'composite-house');
var casaRemover7 = new City('Casa remover 7', 'casa-remover-7', 'composite-house');

liberdade.add(casa1);
liberdade.add(casa2);

ipiranga.add(casa3);
ipiranga.add(casaRemover7);

ipiranga.remove(casaRemover7); // Removido

lapa.add(casa4);

leblon.add(casa5);
leblon.add(casa6);

saoPaulo.add(liberdade);
saoPaulo.add(ipiranga);

rioDeJaneiro.add(lapa);
rioDeJaneiro.add(leblon);

cities.add(saoPaulo);
cities.add(rioDeJaneiro);

document.body.appendChild(cities.getElement());
 
```

Se preferir, aqui está o [demo](http://pedrotcaraujo.github.io/composite-pattern-js), de como ficou o exemplo funcionando.

Composite pattern é interessante quando temos uma coleção de objetos que possuem o mesmo tratamento, é um ótimo pattern e bastante usado pelos desenvolvedores.

Espero que tenha gostado, qualquer dúvida, sugestão, crítica, fique a vontade para deixar seu comentário. Até a próxima :D
