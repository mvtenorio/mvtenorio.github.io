---
layout: post
title:  "Fazendo uma calculadora com Vue.js e Flexbox - Parte 1"
date:   "2015-12-01 18:04:00 -0300"
---

Esta é uma série de posts dividida em três partes:

- CSS - Design da calculadora usando Flexbox
- Javascript - Utilizando o framework Vue.js
- Desenvolvimento e deploy - Webpack, Autoprefixer, Stylus e Jade

Na primeira parte, vou falar sobre CSS. Não sou nenhum especialista em design. Na verdade, ainda sofro bastante pra fazer algo minimamente atraente. Sei bem como é ter uma ideia de como um aplicativo (ou site, sistema, tanto faz) pode parecer mas não saber como transformar isso em CSS. Aí entra o Flexbox, que torna isso bem mais fácil.

O Flexbox é, de acordo com a [especificação do W3C](http://www.w3.org/TR/css3-flexbox/):

> um box model otimizado para design de interfaces de usuário. No modelo de layout flex, os filhos de um flex container podem ser dispostos em qualquer direção, e tem o tamanho flexível para, ou se expandir para preencher o espaço não utilizado, ou encolher para evitar transbordar o elemento pai.

Basicamente, seus conceitos básicos são:

  - Flex container: pode ser qualquer elemento que contenha outros elementos, como div e ul.
  - Flex item: todos os filhos imediatos do Flex Container.

\* Por fins de praticidade, vou usar apenas *container* e *itens* para se referir aos conceitos acima.

Por exemplo, na estrutura seguinte:

```html
<div class="container">
  <input type="button">
  <p>
    <input type="text">
  </p>
</div>

<style>
.container {
  display: flex;
}
</style>
```

Ao aplicarmos `display: flex` em `.container`, este se torna um container e os elementos input button e p se tornam itens, mas o input text não, pois não é filho imediato da div `.container`.

Neste post, mencionarei apenas as características do flexbox relevantes ao que estamos fazendo. Você pode saber mais sobre Flexbox nesse [guia](https://css-tricks.com/snippets/css/a-guide-to-flexbox/) (inglês) e nessa [série de vídeos](http://flexbox.io/) (também em inglês) sobre o assunto.

Vamos ao código, então. O objetivo é criar uma interface parecida com a calculadora do Android:

![Calculadora]({{ site.baseurl }}/images/001/calculadora-google.png "Calculadora")

 O primeiro passo é criar o arquivo *index.html*:

```html
<!DOCTYPE html>
<html lang="en">
<head>
 <meta charset="UTF-8">
 <title>Calculator</title>
 <link rel="stylesheet" href="css/style.css">
</head>
<body>

  <div class="calculator">

    <input type="text" class="expression" autofocus readonly>

    <div class="keyboard">

      <div class="left">
        <input type="button" class="key key0" value="0">
        <input type="button" class="key key1" value="1">
        <input type="button" class="key key2" value="2">
        <input type="button" class="key key3" value="3">
        <input type="button" class="key key4" value="4">
        <input type="button" class="key key5" value="5">
        <input type="button" class="key key6" value="6">
        <input type="button" class="key key7" value="7">
        <input type="button" class="key key8" value="8">
        <input type="button" class="key key9" value="9">
        <input type="button" class="key key-point" value=".">
        <input type="button" class="key key-equals" value="=">
      </div>

      <div class="right">
        <input type="button" class="key" value="←">
        <input type="button" class="key" value="÷">
        <input type="button" class="key" value="×">
        <input type="button" class="key" value="-">
        <input type="button" class="key" value="+">
      </div>
    </div>
  </div>

</body>
</html>
```

Depois, criamos o arquivo CSS referenciado no *index.html* e aplicamos um pouco de estilo para que a calculadora se pareça um pouco mais com o produto final.

```css
/* css/style.css */

.calculator {
  border-radius: 4px;
  border: 2px solid black;
  width: 200px;
}

.expression {
  border: none;
  resize: none;
  font-size: 20px;
  padding: 10px;
  color: #2c3e50;
}

.key {
  border: none;
  background: #34495e;
  color: white;
  font-size: 15px;
  height: 50px;
}

.right .key {
  background: #95a5a6;
}
```

Após criar os dois arquivos, abra o arquivo *index.html* no seu navegador e veja o resultado. Você pode notar que o input `.expression` utrapassa os limites da div `.calculator` devido ao valor colocado na propriedade `font-size`. Isso será corrigido automaticamente ao usar o Flexbox.

Precisamos agora identificar qual elemento será o container principal. O elemento que envolve todos os outros é a div `.calculator`. Vamos então alterar sua propriedade `display`:

```css
.calculator {
  ...
  display: flex;
}
```

Se você testar agora no navegador, verá que o input `.expression` e a div `.keyboard` ficam lado a lado. Esses são os itens do container e o motivo deles se posicionarem dessa forma é a propriedade `flex-direction`, que tem por padrão o valor `row`. Isso significa que os itens se posicionam um ao lado do outro, seguindo uma linha.

A princípio, essa propriedade me confundiu pois achei que o valor `row` deveria colocar cada item em sua própria linha, ou seja, um embaixo do outro. Porém, não é assim que o Flexbox funciona. Essa propriedade determina a direção que os items devem tomar: a direção de uma linha ou a direção de uma coluna.

Temos que alterar o `flex-direction` para `column`:

```css
.calculator {
  ...
  display: flex;
  flex-direction: column;
}
```

Com essa alteração, os items se posicionam um embaixo do outro, na direção de uma coluna. Note que o input `.expression` se adapta ao tamanho do container.

Para posicionar os elementos do teclado da forma que queremos, vamos transformar a div `.keyboard` também em um container, tornando as divs `.left` e `.right` em itens e as posicionando lado a lado. Você sempre pode colocar um container dentro de outro (nesse caso, a div `.keyboard` é um container e um item ao mesmo tempo).

```css
.keyboard {
  display: flex;
  /* aqui omitimos a propriedade flex-direction pois não queremos alterar o valor padrão */
}
```

Descendo mais um nível, fazemos as divs *.left* e *.right* se tornarem containers:

```css
.keyboard .left {
  display: flex;
}

.keyboard .right {
  display: flex;
}
```

Em ambas as divs, os elementos se posicionam em linha, como esperado. No caso da div `.left` precisamos que os itens se dividam em mais linhas para que caibam dentro do container. Fazemos isso com a propriedade `flex-wrap`:

```css
.keyboard .left {
  display: flex;
  flex-wrap: wrap;
}
```

Essa propriedade tem, por padrão, o valor `nowrap` que impede a quebra de linha.

Na div `.right`, podemos alterar a direção para coluna, já que queremos apenas um item (ou seja, um botão) por linha.
Também vamos colocar um tamanho fixo para essa div. Note que os botões se esticam de acordo com o tamanho da coluna.

```css
.keyboard .right {
  width: 55px;
  display: flex;
  flex-direction: column;
}
```

Na coluna da esquerda temos doze itens e precisamos que eles sejam distribuídos em 4 linhas. Para isso, usamos a propriedade `flex` que, por sua vez, é uma abreviação de outras 3 propriedades, na seguinte ordem:

  - `flex-grow`: define a habilidade do item de se expandir caso sobre espaço (padrão: 0).
  - `flex-shrink`: define a habilidade do item de encolher caso falte espaço (padrão: 1).
  - `flex-basis`: define o tamanho padrão do item.

Podemos então, aplicá-la da seguinte maneira:

```css
.key {
  ...
  /* precisamos que o item ocupe 1/3 da linha */
  flex: 1 1 33.333%;
}
```
Essa propriedade é aplicada aos itens e não ao container. Nesse exemplo, todos os itens tem o mesmo valor mas você pode aplicar um valor diferente para um item específico.

****
Apenas para entender como a propriedade `flex-grow` funciona:

  1. altere o terceiro valor de `flex` para 25%. Com isso, 4 itens ficam em cada linha, já que cada um tem o tamanho de 25%.
  2. altere agora para 25.1%. Note que só cabem 3 itens na linha, mas eles se expandem e ocupam a linha inteira.
  3. altere o primeiro valor de `flex` para 0 e veja como os itens ocupam apenas o espaço especificado e o restante fica em branco.

Após isso, volte a propriedade `flex` para o valor correto.

****

Agora a calculadora já está quase pronta, mas a ordem dos botões está diferente da que queremos. Você pode, é claro, simplesmente alterar a ordem dos elementos no html, mas o Flexbox também possui a capacidade de alterar a ordem dos itens com a propriedade `order`.

Ela funciona como um peso. Por padrão, todos os itens tem peso 1 e os itens que tiverem peso maior ficam por último. Para colocar os itens na ordem correta, faça o seguinte:

```css
.key7 { order: 1; }
.key8 { order: 2; }
.key9 { order: 3; }
.key4 { order: 4; }
.key5 { order: 5; }
.key6 { order: 6; }
.key1 { order: 7; }
.key2 { order: 8; }
.key3 { order: 9; }
.key-point { order: 10; }
.key0 { order: 11; }
.key-equals { order: 12; }
```

O resultado final deve ser esse:

![Calculadora final]({{ site.baseurl }}/images/001/calculadora-final.png "Calculadora final")

Concluindo, podemos ver que o Flexbox simplifica a tarefa de criar uma interface usando apenas CSS sem a necessidade de usar propriedades como float e clear, e sem recorrer a frameworks de front-end como o Bootstrap.

Mas pra que serve uma calculadora que não faz nada, né? No segundo post desta série, vou mostrar como implementar as funções de uma calculadora usando o framework Vue.js. Em um terceiro post, vou explicar como adicionar prefixos no CSS para navegadores mais antigos usando o [Autoprefixer](https://github.com/postcss/autoprefixer) com o Webpack como build tool.
