---
layout: post
title: Como os navegadores funcionam 
---

Depois de uma série artigos falando sobre Javascript, hoje gostaria de falar um pouco sobre como os navegadores renderizam uma página web e mostrar alguns princípios da renderização, repaint, reflow e como os browsers optimizam esse processo.

<!--more-->

 Nesse artigo, pretendo não dar muitos detalhes, mas caso você tenha interesse em ir além, a Israelita, Tali Garsiel, fez uma pesquisa bastante aprofundada sobre o assunto, e você pode ver nesse link ["How browsers work: Behind the scenes of modern web browsers"](http://taligarsiel.com/Projects/howbrowserswork1.htm).

 Entender como uma página é renderizada é fundamental, pois possibilita que os desenvolvedores tomem decisões corretas e garante respostas de muitas questões sobre as melhores práticas de desenvolvimento.

 Cada engine dos navegadores funciona de forma diferente e isso requer um estudo especifico de cada uma delas. Porém, vou listar aqui as principais ações de quando a página está sendo renderizada.

 - **DOM:** A engine de renderização irá parsear o HTML localizado no servidor e transformar suas tags em nós do DOM (Document Object Model) em uma tree chamada de "content tree".

 - **CSSOM:** Os estilos são carregados e parseados, formando o CSSOM (CSS Object Model).

 - **Render tree:** A união do DOM com o CSSOM, é criada uma nova tree chamada de "render tree". Que é uma série de objetos para serem renderizados. Render tree contém toda a estrutura do DOM, porém sem os elementos invisíveis (Tag ```<head>``` ou elementos que possuem ```display:none;```).  
 ATENÇÃO: elementos com ```visibility: hidden;``` que ao contrário do ```display:none;``` está no render tree.  
 O render tree é a representação visual do DOM, cada objeto a ser renderizado possui seu objeto do DOM correspondente, junto com seus atributos visuais, como cores, dimensões e vem na ordem correta para ser exibida na tela.

 - **Layout (flow):** O render tree é o suficiente para começar a calcular as coordenadas e posicionar os elementos na tela, esse processo chamamos de "layout". O browser utiliza o método "flow" nesse passo, que é utilizado somente uma vez para todos os elementos, ao contrario das tabelas que exigem sua utilização mais de uma vez.

 - **Painting:** Por final, temos o processo "painting", o render tree irá passar por cada objeto e irá mostrar na tela.



### reflow/repaint

Quando a página está sendo renderizada sabemos que o **layout** e o **painting** é executado pelo menos uma única vez (exceto se você está renderizando uma página em branco). Porém na interação do usuario com a página, scripts podem modificar o DOM e o CSSOM, e então alguns passos a cima podem ser repetidos.

 - **reflow:** Temos um reflow quando algum script modifica o tamanho ou posicionamento dos elementos na página.

 - **repaint:** Quando alterado estilos que não afetam o tamanho ou posicionamento dos elementos na página (```background-color: red```), somente o repaint é disparado. 


#### Exemplos reflow/repaint

```javascript
var bodyStyle = document.body.style;
bodyStyle.width = "500px"; // reflow e repaint
bodyStyle.padding = "10px"; // reflow e repaint
bodyStyle.backgroundColor = "blue"; // repaint
bodyStyle.color = "red"; // repaint

bodyStyle.display = "none"; // reflow e repaint
bodyStyle.visibility = "hidden"; // repaint
```

Executar um reflow ou um repaint, é muito custoso para o browser, portanto ele evita esse processo sempre que pode, funcionando de forma inteligente.

### Optimizando a renderização

Executando esse script, quantos reflows e repaints vamos ter no final?

```javascript
var bodyStyle = document.body.style;

bodyStyle.padding = "10px"; // reflow e repaint
bodyStyle.backgroundColor = "blue"; // repaint
bodyStyle.color = "red"; // repaint
bodyStyle.margin = "10px"; // reflow e repaint

```

Se sua resposta foi mais de 1 vez, você está errado. O browser para optimizar o processo e evitar varios reflows e repaints, guarda todas as instruções e executa tudo de uma só vez no final do script. Legal, não?

Caso seja necessário, você também pode forçar a execução de um reflow e repaint, apenas lendo a propriedade. Veja:


```javascript
var bodyStyle = document.body.style;

bodyStyle.padding = "10px";
bodyStyle.padding; // reflow forçado
bodyStyle.color = "red";
bodyStyle.margin = "10px";

```

No nosso exemplo a cima, temos 2 reflows sendo executado, pois forçamos sua execução quando lemos a propriedade ```padding``` do ```body```.






