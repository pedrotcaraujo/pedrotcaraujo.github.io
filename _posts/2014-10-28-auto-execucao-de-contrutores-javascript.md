---
layout: post
title: Auto-execução de contrutores usando keyword 'new'
---

Existem diversas coisas em JavaScript que deixam a gente de boquiaberta, e diria que isso é um dos motivos que aumenta minha paixão pela linguagem.

<!--more-->

Bom, para quem trabalha com funções JavaScript, ou melhor, ```classes```, está bastante familiarizado com a keyword ```new```. Com a keyword ```new``` você pode passar argumentos na chamada da função. Mas você sabia que caso você não tenha argumentos, podemos não usar parênteses para tudo?

```javascript 
function MinhaClasse() {
    console.log("Inicializada!");
}

var instancia = new MinhaClasse;

// >> "Inicializada!"
```

Ok, é uma coisa um tanto quanto inútil, mas é um truque do JavaScript que podemos guardar com facilidade na nossa cabeça =D.

Richard Ayotte criou até um teste de performance no jsPerf e detectou uma micro performance em browser que utilizam v8. Você pode conferir [aqui](http://jsperf.com/new-with-and-without-parens)

Simples assim, qualquer coisa não esqueçam de usar os comentários.

fonte: Inspirado e traduzido [DavidWalsh](http://davidwalsh.name/javascript-constructor-autoexecution-keyword)