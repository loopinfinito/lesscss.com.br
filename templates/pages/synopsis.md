Variáveis
---------

Variáveis permitem que você especifique valores amplamente utilizados em apenas um local, possibilitando assim reutilizá-las por toda sua folha de estilos,
realizando mudanças globais de maneira tão fácil como alterar apenas uma linha de código.

<table class="code-example" cellpadding="0">
  <tr><td>
  <pre class="less-example">
  <code>// LESS

@color: #4D926F;

#header {
  color: @color;
}
h2 {
  color: @color;
}</code></pre>
  </td><td>
  <pre class="css-output"><code>/* CSS Compilado */

#header {
  color: #4D926F;
}
h2 {
  color: #4D926F;
}</code></pre></td>
  </tr>
</table>
		
Mixins
------

Mixins permitem que você adicione todas as propriedades de uma classe em outra classe
simplesmente incluindo o nome da classe como uma de suas propriedades. Assim como variáveis,
mas para classes completas. Mixins também podem se comportar com funções, e receber argumentos,
como mostrado no exemplo abaixo.

<table class="code-example" cellpadding="0">
  <tr><td>
  <pre class="less-example"><code>// LESS

.rounded-corners (@radius: 5px) {
  border-radius: @radius;
  -webkit-border-radius: @radius;
  -moz-border-radius: @radius;
}

#header {
  .rounded-corners;
}
#footer {
  .rounded-corners(10px);
}</code></pre></td>

<td>
  <pre class="css-output"><code>/* CSS Compilado */

#header {
  border-radius: 5px;
  -webkit-border-radius: 5px;
  -moz-border-radius: 5px;
}
#footer {
  border-radius: 10px;
  -webkit-border-radius: 10px;
  -moz-border-radius: 10px;
}</code></pre>
  </td></tr>
</table>

Regras Aninhadas
----------------

Ao invés de construir longos nomes de seletores para especificar herança,
em Less você pode simplesmente aninhar seletores dentro de outros seletores.
Isto faz com que a herança fique mais clara e as folhas de estilos menores.

<table class="code-example" cellpadding="0">
  <tr><td>
  <pre class="less-example">
<code>// LESS

#header {
  h1 {
    font-size: 26px;
    font-weight: bold;
  }
  p { font-size: 12px;
    a { text-decoration: none;
      &:hover { border-width: 1px }
    }
  }
}

</code></pre></td>

<td>
  <pre class="css-output"><code>/* CSS Compilado */

#header h1 {
  font-size: 26px;
  font-weight: bold;
}
#header p {
  font-size: 12px;
}
#header p a {
  text-decoration: none;
}
#header p a:hover {
  border-width: 1px;
}

</code></pre>
  </td></tr>
</table>
		
Funções & Operações
-------------------

Alguns elementos de sua folha de estilo são proporcionais a outros elementos?
Operações permitem que você some, subtraia, divida e multiplique valores de propriedades e cores,
dando a você o poder de criar relações complexas entre propriedades.
Funções mapeam de um-para-um com código JavaScript, permitindo a você manipular valores
como quiser.

<table class="code-example" cellpadding="0">
  <tr><td>
  <pre class="less-example">
<code>// LESS

@the-border: 1px;
@base-color: #111;
@red:        #842210;

#header {
  color: @base-color * 3;
  border-left: @the-border;
  border-right: @the-border * 2;
}
#footer { 
  color: @base-color + #003300;
  border-color: desaturate(@red, 10%);
}

</code></pre></td>

<td>
  <pre class="css-output"><code>/* CSS Compilado */

#header {
  color: #333;
  border-left: 1px;
  border-right: 2px;
}
#footer { 
  color: #114411;
  border-color: #7d2717;
}

</code></pre>
  </td></tr>
</table>

