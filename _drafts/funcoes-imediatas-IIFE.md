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
	var str = "";
	return function(x) {		
		return str = !!str ?  str.concat(" ", x) : str.concat(x);
	}
})()

adder("Olá"); // Olá
adder("Mundo!"); // Olá Mundo!
```

Criamos uma variável como string dentro do escopo da função imediata e depois retornamos uma função que concatena a string com o valor passado pela função.

Ok, entender seu proposito e o que ela pode nos ajudar até parece fácil, dificil é entender sua sintaxe.
