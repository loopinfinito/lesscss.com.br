Vari&aacute;veis
---------

Vari&aacute;veis permitem que voc&ecirc; especifique valores amplamente utilizados em apenas um local, possibilitando assim reutiliz&aacute;-las por toda sua folha de estilos,
realizando mudan&ccedil;as globais de maneira t&atilde;o f&aacute;cil como alterar apenas uma linha de c&oacute;digo.

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

Mixins permitem que voc&ecirc; adicione todas as propriedades de uma classe em outra classe
simplesmente incluindo o nome da classe como uma de suas propriedades. Assim como vari&aacute;veis,
mas para classes completas. Mixins tamb&eacute;m podem se comportar com fun&ccedil;&otilde;es, e receber argumentos,
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

Ao inv&eacute;s de construir longos nomes de seletores para especificar heran&ccedil;a,
em Less voc&ecirc; pode simplesmente aninhar seletores dentro de outros seletores.
Isto faz com que a heran&ccedil;a fique mais clara e as folhas de estilos menores.

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
      &amp;:hover { border-width: 1px }
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
		
Fun&ccedil;&otilde;es &amp; Opera&ccedil;&otilde;es
-------------------

Alguns elementos de sua folha de estilo s&atilde;o proporcionais a outros elementos?
Opera&ccedil;&otilde;es permitem que voc&ecirc; some, subtraia, divida e multiplique valores de propriedades e cores,
dando a voc&ecirc; o poder de criar rela&ccedil;&otilde;es complexas entre propriedades.
Fun&ccedil;&otilde;es mapeam de um-para-um com c&oacute;digo JavaScript, permitindo a voc&ecirc; manipular valores
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

