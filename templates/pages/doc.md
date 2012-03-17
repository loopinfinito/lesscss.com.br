Como uma extens&atilde;o do CSS, LESS n&atilde;o &eacute; somente retrocompat&iacute;vel com CSS, mas as funcionalidades extras que ele adicona utilizam a sintaxe <em>existente</em> do CSS. Isso torna o aprendizado do LESS <em>f&aacute;cil</em> e, em caso de d&uacute;vida, permite que voc&ecirc; use a sintaxe regular de CSS como fallback.

Vari&aacute;veis
---------

Estes se auto explicam:

    @nice-blue: #5B83AD;
    @light-blue: @nice-blue + #111;

    #header { color: @light-blue; }

Sa&iacute;da:

    #header { color: #6c94be; }

Tamb&eacute;m &eacute; poss&iacute;vel definir vari&aacute;veis com um nome vari&aacute;vel:

    @fnord: 'I am fnord.';
    @var: "fnord";
    content: @@var;

Que compila para:

    content: 'I am fnord.';

Na verdade as vari&aacute;veis em LESS s&atilde;o "constantes", pois elas s&oacute; podem ser definidas uma vez.

Mixins
------

Em LESS, &eacute; poss&iacute;vel incluir v&aacute;rias propriedades de um ruleset em um outro ruleset. Digamos que n&oacute;s temos a seguinte classe:

    .bordered {
      border-top: dotted 1px black;
      border-bottom: solid 2px black;
    }

E n&oacute;s queremos usar essas propriedades dentro de outros rulesets. N&oacute;s apenas temos que por o nome da classe dentro de qualquer ruleset que desejamos utilizar estas propriedades, como no exemplo:

    #menu a {
      color: #111;
      .bordered;
    }
    .post a {
      color: red;
      .bordered;
    }

As propriedades da classe `.bordered` agora v&atilde;o aparecer tanto em `#menu a` como em `.post a`:

    #menu a {
      color: #111;
      border-top: dotted 1px black;
      border-bottom: solid 2px black;
    }
    .post a {
      color: red;
      border-top: dotted 1px black;
      border-bottom: solid 2px black;
    }

Qualquer ruleset CSS do tipo *classe* ou *id* pode ser usado como mixin desta forma.

Mixins Param&eacute;tricos
-------------------

LESS possui um tipo especial de ruleset que pode ser usado como mixin, como as classes, mas que aceitam par&acirc;metros. Este &eacute; o exemplo can&ocirc;nico:

    .border-radius (@radius) {
      border-radius: @radius;
      -moz-border-radius: @radius;
      -webkit-border-radius: @radius;
    }

E aqui como podemos us&aacute;-lo como mixin em v&aacute;rios rulesets:

    #header {
      .border-radius(4px);
    }
    .button {
      .border-radius(6px);
    }

Mixins param&eacute;tricos podem inclusive ter valores padr&atilde;o para seus par&acirc;metros:

    .border-radius (@radius: 5px) {
      border-radius: @radius;
      -moz-border-radius: @radius;
      -webkit-border-radius: @radius;
    }

Agora n&oacute;s podemos usar o mixin anterior como:

    #header {
      .border-radius;
    }

E ele incluir&aacute; um border-radius de 5px.

Voc&ecirc; pode inclusive usar mixins param&eacute;tricos que n&atilde;o recebem par&acirc;metros. Isto &eacute; &uacute;til se voc&ecirc; quiser esconder o ruleset do da sa&iacute;da em CSS,
mas deseja incluir as propriedades em outros rulesets:

    .wrap () {
      text-wrap: wrap;
      white-space: pre-wrap;
      white-space: -moz-pre-wrap;
      word-wrap: break-word;
    }

    pre { .wrap }

Que gera a sa&iacute;da:

    pre {
      text-wrap: wrap;
      white-space: pre-wrap;
      white-space: -moz-pre-wrap;
      word-wrap: break-word;
    }

### A vari&aacute;vel `@arguments`

`@arguments` possui um significado especial dentro de mixins. Ele possui todos os argumentos passados quando o mixin foi chamado.
Isto &eacute; &uacute;til se voc&ecirc; n&atilde;o quer se preocupar individualmente com cada par&acirc;metro.

    .box-shadow (@x: 0, @y: 0, @blur: 1px, @color: #000) {
      box-shadow: @arguments;
      -moz-box-shadow: @arguments;
      -webkit-box-shadow: @arguments;
    }
    .box-shadow(2px, 5px);

O que gera a sa&iacute;da:

      box-shadow: 2px 5px 1px #000;
      -moz-box-shadow: 2px 5px 1px #000;
      -webkit-box-shadow: 2px 5px 1px #000;

## Pattern-matching e express&otilde;es Guard

Voc&ecirc; pode querer modificar o comportamento de um mixin baseado nos par&acirc;metros passados.
Vamos come&ccedil;ar com algo b&aacute;sico

    .mixin (@s, @color) { ... }

    .class {
      .mixin(@switch, #888);
    }

Agora digamos que n&oacute;s queremos que `.mixin` tenha um comportamento diferente, baseado no valor de `@switch`.
N&oacute;s poderiamos definir `.mixin` como:

    .mixin (dark, @color) {
      color: darken(@color, 10%);
    }
    .mixin (light, @color) {
      color: lighten(@color, 10%);
    }
    .mixin (@_, @color) {
      display: block;
    }

Agora, se rodarmos:

    @switch: light;

    .class {
      .mixin(@switch, #888);
    }

N&oacute;s vamos obter o seguinte CSS:

    .class {
      color: #a2a2a2;
      display: block;
    }

Onde a cor passada em `.mixin` foi clareada. Se o valor de `@switch` fosse `dark`, o resultado
teria sido uma cor escurecida.

O que aconteceu, passo-a-passo:

- A primeira defini&ccedil;&atilde;o do mixin n&atilde;o se encaixava, pois era esperado o valor `dark` como primeiro argumento.
- A segunda defini&ccedil;&atilde;o do mixin encaixa, pois era esperado `light`.
- A terceira defini&ccedil;&atilde;o do mixin tamb&eacute;m encaixa, pois ele espera por qualquer valor.

Apenas as defini&ccedil;&otilde;es de mixins que foram encaixadas (matched) foram usadas. Vari&aacute;veis encaixam e ficam vinculadas a qualquer valor.
Qualquer coisa que n&atilde;o seja uma vari&aacute;vel se encaixa apenas com um valor igual a si mesmo.

&Eacute; poss&iacute;vel tamb&eacute;m dar match pela aridade (n&uacute;mero de argumentos). Aqui vai um exemplo:

    .mixin (@a) {
      color: @a;
    }
    .mixin (@a, @b) {
      color: fade(@a, @b);
    }

Agora se chamarmos `.mixin` com apenas um argumento, vamos ter a sa&iacute;da da primeira defini&ccedil;&atilde;o,
mas se chamarmos `.mixin` com *dois* argumentos, o match vai acontecer na segunda defini&ccedil;&atilde;o, que pode ser traduzido para `@a` da um fade para `@b`.

### Guards

Guards s&atilde;o &uacute;teis quando se quer dar match em *expres&otilde;es*, e n&atilde;o em simples valores ou aridade.
Se voc&ecirc; &eacute; familiar com programa&ccedil;&atilde;o funcional, voc&ecirc; provavelmente j&aacute; se deparou com ela.

Na tentative de permanecer o mais pr&oacute;ximo poss&iacute;vel da natureza declarativa do CSS, LESS optou por implementar
execu&ccedil;&atilde;o condicional atrav&eacute;s de **guarded mixins** ao inv&eacute;s de if/else, seguindo a especifica&ccedil;&atilde;o do `@media` query.

Vamos come&ccedil;ar com um exemplo:

    .mixin (@a) when (lightness(@a) &gt;= 50%) {
      background-color: black;
    }
    .mixin (@a) when (lightness(@a) &lt; 50%) {
      background-color: white;
    }
    .mixin (@a) {
      color: @a;
    }

O ponto crucial aqui &eacute; a palavra reservada **`when`**, que introduz uma sequ&ecirc;ncia guard (aqui com apenas um guard).
Agora se rodarmos o seginte c&oacute;digo:

    .class1 { .mixin(#ddd) }
    .class2 { .mixin(#555) }


Isto &eacute; o que vamos obter:

    .class1 {
      background-color: black;
      color: #ddd;
    }
    .class2 {
      background-color: white;
      color: #555;
    }

A lista completa de operadores de compara&ccedil;&atilde;o usadas nos guards s&atilde;o: **`&gt; &gt;= = =&lt; &lt;`**. Adicionalmente, a palavra reservada `true` &eacute; o &uacute;nico valor verdade em uma compara&ccedil;&atilde;o, fazendo esses dois mixins equivalentes:

    .truth (@a) when (@a) { ... }
    .truth (@a) when (@a = true) { ... }

Qualquer outro valor que n&atilde;o seja a palavra reservada `true` &eacute; falso em uma compara&ccedil;&atilde;o:

    .class {
      .truth(40); // Não irá dar match em nenhuma das definições acima.
    }

Guards tamb&eacute;m pode ser separados por v&iacute;rgula "`,`"--se algum guard for avaliado como verdadeiro, ent&atilde;o &eacute; dado o match:

    .mixin (@a) when (@a &gt; 10), (@a &lt; -10) { ... }

Tamb&eacute;m &eacute; poss&iacute;vel comparar argumentos com outros argumentos, ou com n&atilde;o-argumentos:

    @media: mobile;

    .mixin (@a) when (@media = mobile) { ... }
    .mixin (@a) when (@media = desktop) { ... }

    .max (@a, @b) when (@a &gt; @b) { width: @a }
    .max (@a, @b) when (@a &lt; @b) { width: @b }

E por &uacute;ltimo, se voc&ecirc; quiser dar match em mixins baseado em valores de tipos, &eacute; poss&iacute;vel usar as fun&ccedil;&otilde;es *is\**:

    .mixin (@a, @b: 0) when (isnumber(@b)) { ... }
    .mixin (@a, @b: black) when (iscolor(@b)) { ... }

Aqui est&atilde;o as fun&ccedil;&otilde;es b&aacute;sicas de checagem de tipos:

- `iscolor`
- `isnumber`
- `isstring`
- `iskeyword`
- `isurl`

Se voc&ecirc; quiser checar se um valor, al&eacute;m de ser um n&uacute;mero, est&aacute; em uma unidade espec&iacute;fica, voc&ecirc; pode usar:

- `ispixel`
- `ispercentage`
- `isem`

Al&eacute;m disso tudo, ainda &eacute; poss&iacute;vel usar a palavra reservada **`and`** para condi&ccedil;&otilde;es adicionais dentro de um guard:

    .mixin (@a) when (isnumber(@a)) and (@a &gt; 0) { ... }

E a palavra reservada **`not`** para negar as condi&ccedil;&otilde;es:

    .mixin (@b) when not (@b &gt; 0) { ... }

Regras aninhadas
----------------

LESS lhe d&aacute; a habilidade de usar *aninhamento*, com ou sem cascateamento (cascading).
Digamos que temos o seguinte CSS:

    #header { color: black; }
    #header .navigation {
      font-size: 12px;
    }
    #header .logo {
      width: 300px;
    }
    #header .logo:hover {
      text-decoration: none;
    }

Em LESS, n&oacute;s tamb&eacute;m podemos escrever da seguinte maneira:

    #header {
      color: black;

      .navigation {
        font-size: 12px;
      }
      .logo {
        width: 300px;
        &amp;:hover { text-decoration: none }
      }
    }

Ou desta outra maneira:

    #header        { color: black;
      .navigation  { font-size: 12px }
      .logo        { width: 300px;
        &amp;:hover    { text-decoration: none }
      }
    }

O c&oacute;digo resultante &eacute; mais conciso, e mimica a estrutura da sua `&aacute;rvore DOM`.

Perceba o combinador `&amp;`--ele &eacute; usado quando queremos usar um seletor aninhado concatenado aos seu seletor pai, ao contr&aacute;rio de atuar como um descendente.
Isto &eacute; muito importante para pseudo-classe como `:hover` e `:focus`.

Por exemplo:

    .bordered {
      &amp;.float {
        float: left;
      }
      .top {
        margin: 5px;
      }
    }

Vai gerar a sa&iacute;da:

    .bordered.float {
      float: left;
    }
    .bordered .top {
      margin: 5px;
    }

Opera&ccedil;&otilde;es
----------

Qualquer n&uacute;mero, cor ou vari&aacute;vel pode ser usado em opera&ccedil;&otilde;es. Alguns exemplos:

    @base: 5%;
    @filler: @base * 2;
    @other: @base + @filler;

    color: #888 / 4;
    background-color: @base-color + #111;
    height: 100% / 2 + @filler;

A sa&iacute;da &eacute; basicamente o que se espera-LESS entende a diferen&ccedil;a entre cores e unidade. Se uma unidade &eacute; usada em uma opera&ccedil;&atilde;o, como em:

    @var: 1px + 5;

LESS ir&aacute; usar esta unidade para a sa&iacute;da final-`6px` neste caso.

Par&ecirc;nteses tamb&eacute;m s&atilde;o permitidos nessas opera&ccedil;&otilde;es:

    width: (@var + 5) * 2;

E s&atilde;o obrigat&oacute;rios em valores compostos:

    border: (@width * 2) solid black;

Func&otilde;es de cores
----------------

LESS fornece uma variedade de fun&ccedil;&otilde;es que transformam cores. Cores s&atilde;o primeiramente convertidas para
o formato *HSL*, e ent&atilde;o manipuladas a n&iacute;vel de canal:

    lighten(@color, 10%);     // retorna uma cor que é 10% *mais clara* que @color
    darken(@color, 10%);      // retorna uma cor que é 10% *mais escura* que @color

    saturate(@color, 10%);    // retorna uma cor 10% *mais* saturada que @color
    desaturate(@color, 10%);  // retorna uma cor 10% *menos* saturada que @color

    fadein(@color, 10%);      // retorna uma cor 10% *mais* transparente que @color
    fadeout(@color, 10%);     // retorna uma cor 10% *more* transparente que @color
    fade(@color, 50%);        // retorna @color com 50% de transparencia

    spin(@color, 10);         // retorna uma cor com uma matiz (hue) 10 graus maior que @color
    spin(@color, -10);        // retorna uma cor com uma matiz (hue) 10 graus menor que @color

    mix(@color1, @color2);    // retorna uma mistura entre a @color1 e @color2

O uso das fun&ccedil;&otilde;es &eacute; bem simples:

    @base: #f04615;

    .class {
      color: saturate(@base, 5%);
      background-color: lighten(spin(@base, 8), 25%);
    }

Tamb&eacute;m &eacute; poss&iacute;vel extrair informa&ccedil;&atilde; das cores:

    hue(@color);        // retorna o canal `matiz` (hue) de @color
    saturation(@color); // retorna o canal `saturação` (saturation) de @color
    lightness(@color);  // retorna o canal `luminosidade` (lightness) de @color
    alpha(@color);      // retorna o canal "alpha" de @color

Isto &eacute; &uacute;til se quisermos criar uma nova cor baseada em um canal de uma outra cor, por exemplo:

    @new: hsl(hue(@old), 45%, 90%);

`@new` vai ter o *hue* de `@old`, e sua pr&oacute;pria saturation e lightness.

Fun&ccedil;&otilde;es Matem&aacute;ticas
-------------------

LESS fornece algumas fun&ccedil;&otilde;es matem&aacute;ticas bastante &uacute;teis que podemos usar com valores:

    round(1.67); // retorna `2`
    ceil(2.4);   // retorna `3`
    floor(2.6);  // retorna `2`

Se precisarmos transformar um valor em uma porcentagem, poder fazer isso com a fun&ccedil;&atilde;o `percentage`:

    percentage(0.5); // retorna `50%`

Namespaces
----------

Algumas vezes, queremos agrupar nossas vari&aacute;veis e mixins, por motivos de organiza&ccedil;&atilde;o, ou apenas para oferecer encapsulamento.
Podemos fazer isso de forma intuitiva em LESS-digamos que queremos criar um pacote com alguns mixins e vari&aacute;veis com o nome `#bundle`, para re-uso posterior ou para distribui&ccedil;&atilde;o:

    #bundle {
      .button () {
        display: block;
        border: 1px solid black;
        background-color: grey;
        &amp;:hover { background-color: white }
      }
      .tab { ... }
      .citation { ... }
    }

Agora se quisermos adicionar o classe `.button` do mixin ao nosso `#header a`, podemos fazer da seguinte forma:

    #header a {
      color: orange;
      #bundle &gt; .button;
    }

Escopo
------

Escopo em LESS &eacute; muito semelhante ao de linguagens de programa&ccedil;&atilde;o. Vari&aacute;veis e mixins s&atilde;o primeiro procurados localmente, e caso n&atilde;o sejam encontrados, o compilador ir&aacute; procurar no escopo pai, e assim por diante.

    @var: red;

    #page {
      @var: white;
      #header {
        color: @var; // white
      }
    }

    #footer {
      color: @var; // red
    }

Coment&aacute;rios
-----------

Coment&aacute;rios seguindo o padr&atilde;o CSS s&atilde;o preservados em LESS:

    /* Olá, sou um comentário no estilo CSS */
    .class { color: black }

Coment&aacute;rio de uma linha tamb&eacute;m s&atilde;o v&aacute;lidos no LESS, mas s&atilde;o "silenciosos",
eles n&atilde;o aparecem no CSS compilado:

    // Opa, sou um comentário do tipo silencioso e não vou aparecer no CSS final
    .class { color: white }

Importa&ccedil;&atilde;o
----------

Podemos importar arquivos `.less`, e todas as vari&aacute;veis e mixins nestes estar&atilde;o dispon&iacute;veis no arquivo principal.
A extens&atilde;o `.less` &eacute; opcional, ent&atilde;o ambas as regras s&atilde;o v&aacute;lidas:

    @import 'lib.less';
    @import 'lib';

Se quisermos importar um arquivo CSS, e n&atilde;o quisermos que LESS processe ele, apenas usamos a extens&atilde;o `.css`:

    @import 'lib.css';
A diretiva ir&aacute; ficar como est&aacute;, e acabar&aacute; como uma sa&iacute;da CSS.

Interpola&ccedil;&atilde;o de strings
-----------------------

Vari&aacute;veis podem ser inclu&iacute;das dentro de strings de uma maneira similar a ruby ou PHP, com o construtor `@{name}`

    @base-url: 'http://assets.fnord.com';
    background-image: url('@{base-url}/images/bg.png');

Escaping
--------

Podemos precisar usar um valor CSS na sa&iacute;da que n&atilde;o &eacute; v&aacute;lido pela sintaxe do CSS,
ou utiliza uma sintaxe propriet&aacute;ria que LESS n&atilde;o reconhece.

Para gerar a sa&iacute;da de tal valor, o colocamos dentro de uma string come&ccedil;ando com `~`, por exemplo:

    .class {
      filter: ~'ms:alwaysHasItsOwnSyntax.For.Stuff()';
    }

Isto &eacute; chamado de 'escaped valur', que ir&aacute; resultar em:

    .class {
      filter: ms:alwaysHasItsOwnSyntax.For.Stuff();
    }

Avalia&ccedil;&atilde;o (evaluation) de JavaScript
------------------------------------

Express&otilde;es JavaScript podem ser avaliadas (evaluated) como valores detro dos arquivos .less.
Isto &eacute; feito colocando a express&otilde;es entre crases:

    @var: `'hello'.toUpperCase() + "!"`;

Gera a sa&iacute;da:

    @var: 'HELLO!';

Tamb&eacute;m &eacute; poss&iacute;vel usar interpola&ccedil;&atilde;o e escaping com as strings:

    @str: 'hello';
    @var: ~`'@{str}'.toUpperCase() + "!"`;

Gera a sa&iacute;da:

    @var: HELLO!;

&Eacute; poss&iacute;vel inclusive acessar o ambiente JavaScript:

    @height: `document.body.clientHeight`;

Se quisermos dar um parse em uma string JavaScript como uma cor hexadecimal, podemos utilizar a fun&ccedil;&atilde;o `color`:

    @color: color(`window.colors.baseColor`);
    @darkcolor: darken(@color, 10%);
