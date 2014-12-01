---
layout: post
title: Funções imediatas JavaScript (IIFE)
---

Há tempos venho aprendendo e tentando entender como alguns truques em Javascript funcionam, o **IIFE** é um desses caras que muita vezes usamos e não sabemos o que acontece realmente. Esse trecho de código é bastante usado no Javascript funcional, resolve alguns problemas e você pode vê-lo a seguir.

<!--more-->

```javascript
(function(){})();
```

O **IIFE significa "Immediately-invoked function expression"**, ou então podemos chamar de função imediata. Como o próprio nome diz, ela executa a função imediatamente depois que criada, mas por que usar?

### Por que usar funções imediatas?

Encapsulamento! Tenha em mente que variáveis em Javascript têm como escopo a função da qual elas foram definidas (podem ser acessadas somente dentro da função, jamais fora). Ao criar uma função anônima com execução imediata, podemos criar um escopo temporário para nossas funções e variáveis. Com isso evitamos poluição no nosso escopo global e possíveis conflitos de variáveis ou funções com o mesmo nome.

#### Exemplo: Considere o código a seguir

```javascript
var adder = (function() {
	var myPhrase = "";
	return function(x) { 
		return myPhrase = 
			!!myPhrase ? myPhrase.concat(" ", x) : myPhrase.concat(x);
	}
})();

adder("Olá"); // "Olá"
adder("Mundo!"); // "Olá Mundo!"

myPhrase; // myPhrase is not defined
```

Em nosso exemplo, criamos uma **função anônima imediata** que retorna uma outra função que concatena uma string na variável chamada ```myPhrase```. Note que a variável criada ```myPhrase``` está no escopo da **IIFE** e não na função retornada. Portanto o ```myPhrase``` não é definido toda vez que invocamos a função ```adder```. Mas, o mais importante disso tudo é, que o ```myPhrase``` está limitado a escopo da função anônima imediata, não permitindo o seu acesso direto de maneira alguma.

Ok, agora que vimos como funciona uma IIFE, vamos entender sua sintaxe.

### Sintaxe da IIFE
Vamos analisar essa estrutura de parênteses.

```javascript
(...)();
```

É confuso tentar entender a sintaxe de uma IIFE que possui parênteses com comportamentos tão diferentes. Por isso, vamos prosseguir devagar.

Sabemos que podemos definir uma função dessa maneira ```function doSomething() { /* codigo */ }```

Até aqui está lindo, mas e se eu colocar o conjunto de parênteses ```()``` para invocar a função enquanto definimos, aí temos a função imediata, certo?

```javascript
function doSomething() { /* codigo */ }(); // SyntaxError: Unexpected token )

// Agora com função anônima
function() { /* codigo */ }(); // SyntaxError: Unexpected token (

```

Errado! Como você pode ver, se tentamos invocar uma função enquanto definimos temos um erro.

De acordo com a norma do **ExpressionStatement** (estado de expressão), não podemos começar com a keyword ```function```, pois quando usada em escopo global ou dentro de outra função, temos um estado de declaração de uma função ([Function Declaration](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function)). Portanto se tentarmos definir uma IIFE da maneira mostrada acima (iniciando com a keyword ```function```), estaremos lidando com declaração e o que precisamos é de uma expressão ([Function Expression](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/function)).

No nosso exemplo, se por um lado o parser identifica o keyword ```function``` como uma declaração, por outro ele reconhece o parênteses ```()``` como um grupo de operadores e quando utilizamos sem uma expressão dentro temos um erro.

Mas se transformarmos uma função em **Function Expression** e em seguida um conjunto de parênteses esses mesmos parênteses se torna uma invocação de uma função onde podemos passar parâmetros sem problemas, tendo outro comportamento. Como fazer uma function se tornar uma expressão? Apenas inserimos ela dentro de um grupo de operadores.

```javascript
(function doSomething() { /* codigo */ })(); // undefined

// Agora com função anônima
(function() { /* codigo */ })(); // undefined
```
#### Utilizando parâmetros:
```javascript
//Função imediata com parâmetro
(function doSomething(x) { console.log(x); })(1); // 1

// Agora com função anônima
(function(x) { console.log(x) })(1); // 1

```

#### Outras maneiras:

```javascript
(function() { /* codigo */ }()); // Método de Douglas Crockford

```

Em alguns casos o parser já espera uma **Function Expression**, então, não precisamos colocar o grupo de operadores.

```javascript

var doSomething = function() { return "done"; }(); // Sem o grupo de operadores

```

Podemos também definir usando o construtor ```new``` , mas é menos performático que os outros métodos, eu não recomendaria essa opção.

```javascript
new function(){ /* codigo */ };
new function(x, y){ /* codigo */ }(1, 2); // Use parênteses se tiver argumentos.

```
Economizamos um byte transformando para Function Expression sem usar o grupo de operadores e sim com operadores unários.

```javascript
!function(){ /* codigo */ }();
~function(){ /* codigo */ }();
-function(){ /* codigo */ }();
+function(){ /* codigo */ }();

```

Para ir mais além, segue algumas referências:

* [IIFE](http://benalman.com/news/2010/11/immediately-invoked-function-expression/#iife);
* [Every possible way to define a javascript function](http://www.bryanbraun.com/2014/11/27/every-possible-way-to-define-a-javascript-function);

Espero ter ajudado! Dúvidas, sugestões, críticas, só comentar logo abaixo.