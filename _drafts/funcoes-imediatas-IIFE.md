---
layout: post
title: Funções imediatas (IIFE)
---

Há tempos venho aprendendo e tentando entender como alguns truques em Javascript funcionam, o **IIFE** é um desses caras que muita vezes usamos e não sabemos o que acontece realmente. Esse trecho de código é bastante usado no Javascript funcional, resolve alguns problemas e você pode vê-lo a seguir...

<!--more-->

```javascript
(function(){})()
```

O **IIFE significa "Immediately-invoked function expression"**, ou então podemos chamar de função imediata. Como o proprio nome diz, ela executa a função imediatamente depois que criada, legal, não? Mas por que usar?

### Por que usar funções imediatas?

Tenha em mente que variáveis em Javascript têm como escopo a função da qual elas foram definidas (podem ser acessadas somente dentro da função, jamais fora). Ao criar uma função anônima com exeucação imediata, podemos criar um escopo temporario para nossas funções e variaveis. Com isso evitamos poluição no nosso escopo global e possiveis conflitos de variaveis ou funções com o mesmo nome.

#### Exemplo: Considere o código a seguir

```javascript
var adder = (function() {
	var myPhrase = "";
	return function(x) {		
		return myPhrase = !!myPhrase ?  myPhrase.concat(" ", x) : myPhrase.concat(x);
	}
})()

adder("Olá"); // "Olá"
adder("Mundo!"); // "Olá Mundo!"
```

Em nosso exemplo, criamos uma função anônima imediata que retorna uma função que concatena uma string na variavel chamada ```myPhrase```. Note que a variável criada ```myPhrase``` está no escopo da IIFE e não na função retornada. Portanto o ```myPhrase``` não é definido toda vez que invocamos a função ```adder```, e então conseguimos nosso objetivo de concatenar nossas strings. Mas, o mais importante disso tudo é, que o ```myPhrase``` está limitado a escopo da função anônima imediata, não permitindo o seu acesso direto de maneira alguma.

Ok, entender seu proposito e o que ela pode nos ajudar até parece fácil, dificil é entender sua sintaxe.
