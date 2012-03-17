Como uma extensão do CSS, LESS não é somente retrocompatível com CSS, mas as funcionalidades extras que ele adicona utilizam a sintaxe <em>existente</em> do CSS. Isso torna o aprendizado do LESS <em>fácil</em> e, em caso de dúvida, permite que você use a sintaxe regular de CSS como fallback.

Variáveis
---------

Estes se auto explicam:

    @nice-blue: #5B83AD;
    @light-blue: @nice-blue + #111;

    #header { color: @light-blue; }

Saída:

    #header { color: #6c94be; }

Também é possível definir variáveis com um nome variável:

    @fnord: "I am fnord.";
    @var: 'fnord';
    content: @@var;

Que compila para:

    content: "I am fnord.";

Na verdade as variáveis em LESS são 'constantes', pois elas só podem ser definidas uma vez.

Mixins
------

Em LESS, é possível incluir várias propriedades de um ruleset em um outro ruleset. Digamos que nós temos a seguinte classe:

    .bordered {
      border-top: dotted 1px black;
      border-bottom: solid 2px black;
    }

E nós queremos usar essas propriedades dentro de outros rulesets. Nós apenas temos que por o nome da classe dentro de qualquer ruleset que desejamos utilizar estas propriedades, como no exemplo:

    #menu a {
      color: #111;
      .bordered;
    }
    .post a {
      color: red;
      .bordered;
    }

As propriedades da classe `.bordered` agora vão aparecer tanto em `#menu a` como em `.post a`:

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

Mixins Paramétricos
-------------------

LESS possui um tipo especial de ruleset que pode ser usado como mixin, como as classes, mas que aceitam parâmetros. Este é o exemplo canônico:

    .border-radius (@radius) {
      border-radius: @radius;
      -moz-border-radius: @radius;
      -webkit-border-radius: @radius;
    }

E aqui como podemos usá-lo como mixin em vários rulesets:

    #header {
      .border-radius(4px);
    }
    .button {
      .border-radius(6px);
    }

Mixins paramétricos podem inclusive ter valores padrão para seus parâmetros:

    .border-radius (@radius: 5px) {
      border-radius: @radius;
      -moz-border-radius: @radius;
      -webkit-border-radius: @radius;
    }

Agora nós podemos usar o mixin anterior como:

    #header {
      .border-radius;
    }

E ele incluirá um border-radius de 5px.

Você pode inclusive usar mixins paramétricos que não recebem parâmetros. Isto é útil se você quiser esconder o ruleset do da saída em CSS,
mas deseja incluir as propriedades em outros rulesets:

    .wrap () {
      text-wrap: wrap;
      white-space: pre-wrap;
      white-space: -moz-pre-wrap;
      word-wrap: break-word;
    }

    pre { .wrap }

Que gera a saída:

    pre {
      text-wrap: wrap;
      white-space: pre-wrap;
      white-space: -moz-pre-wrap;
      word-wrap: break-word;
    }

### A variável `@arguments`

`@arguments` possui um significado especial dentro de mixins. Ele possui todos os argumentos passados quando o mixin foi chamado.
Isto é útil se você não quer se preocupar individualmente com cada parâmetro.

    .box-shadow (@x: 0, @y: 0, @blur: 1px, @color: #000) {
      box-shadow: @arguments;
      -moz-box-shadow: @arguments;
      -webkit-box-shadow: @arguments;
    }
    .box-shadow(2px, 5px);

O que gera a saída:

      box-shadow: 2px 5px 1px #000;
      -moz-box-shadow: 2px 5px 1px #000;
      -webkit-box-shadow: 2px 5px 1px #000;

## Pattern-matching e expressões Guard

Você pode querer modificar o comportamento de um mixin baseado nos parâmetros passados.
Vamos começar com algo básico

    .mixin (@s, @color) { ... }

    .class {
      .mixin(@switch, #888);
    }

Agora digamos que nós queremos que `.mixin` tenha um comportamento diferente, baseado no valor de `@switch`.
Nós poderiamos definir `.mixin` como:

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

Nós vamos obter o seguinte CSS:

    .class {
      color: #a2a2a2;
      display: block;
    }

Onde a cor passada em `.mixin` foi clareada. Se o valor de `@switch` fosse `dark`, o resultado
teria sido uma cor escurecida.

O que aconteceu, passo-a-passo:

- A primeira definição do mixin não se encaixava, pois era esperado o valor `dark` como primeiro argumento.
- A segunda definição do mixin encaixa, pois era esperado `light`.
- A terceira definição do mixin também encaixa, pois ele espera por qualquer valor.

Apenas as definições de mixins que foram encaixadas (matched) foram usadas. Variáveis encaixam e ficam vinculadas a qualquer valor.
Qualquer coisa que não seja uma variável se encaixa apenas com um valor igual a si mesmo.

É possível também dar match pela aridade (número de argumentos). Aqui vai um exemplo:

    .mixin (@a) {
      color: @a;
    }
    .mixin (@a, @b) {
      color: fade(@a, @b);
    }

Agora se chamarmos `.mixin` com apenas um argumento, vamos ter a saída da primeira definição,
mas se chamarmos `.mixin` com *dois* argumentos, o match vai acontecer na segunda definição, que pode ser traduzido para `@a` da um fade para `@b`.

### Guards

Guards são úteis quando se quer dar match em *expresões*, e não em simples valores ou aridade.
Se você é familiar com programação funcional, você provavelmente já se deparou com ela.

Na tentative de permanecer o mais próximo possível da natureza declarativa do CSS, LESS optou por implementar
execução condicional através de **guarded mixins** ao invés de if/else, seguindo a especificação do `@media` query.

Vamos começar com um exemplo:

    .mixin (@a) when (lightness(@a) >= 50%) {
      background-color: black;
    }
    .mixin (@a) when (lightness(@a) < 50%) {
      background-color: white;
    }
    .mixin (@a) {
      color: @a;
    }

O ponto crucial aqui é a palavra reservada **`when`**, que introduz uma sequência guard (aqui com apenas um guard).
Agora se rodarmos o seginte código:

    .class1 { .mixin(#ddd) }
    .class2 { .mixin(#555) }


Isto é o que vamos obter:

    .class1 {
      background-color: black;
      color: #ddd;
    }
    .class2 {
      background-color: white;
      color: #555;
    }

A lista completa de operadores de comparação usadas nos guards são: **`> >= = =< <`**. Adicionalmente, a palavra reservada `true` é o único valor verdade em uma comparação, fazendo esses dois mixins equivalentes:

    .truth (@a) when (@a) { ... }
    .truth (@a) when (@a = true) { ... }

Qualquer outro valor que não seja a palavra reservada `true` é falso em uma comparação:

    .class {
      .truth(40); // Will not match any of the above definitions.
    }

Guards também pode ser separados por vírgula '`,`'--se algum guard for avaliado como verdadeiro, então é dado o match:

    .mixin (@a) when (@a > 10), (@a < -10) { ... }

Também é possível comparar argumentos com outros argumentos, ou com não-argumentos:

    @media: mobile;

    .mixin (@a) when (@media = mobile) { ... }
    .mixin (@a) when (@media = desktop) { ... }

    .max (@a, @b) when (@a > @b) { width: @a }
    .max (@a, @b) when (@a < @b) { width: @b }

E por último, se você quiser dar match em mixins baseado em valores de tipos, é possível usar as funções *is\**:

    .mixin (@a, @b: 0) when (isnumber(@b)) { ... }
    .mixin (@a, @b: black) when (iscolor(@b)) { ... }

Aqui estão as funções básicas de checagem de tipos:

- `iscolor`
- `isnumber`
- `isstring`
- `iskeyword`
- `isurl`

Se você quiser checar se um valor, além de ser um número, está em uma unidade específica, você pode usar:

- `ispixel`
- `ispercentage`
- `isem`

Além disso tudo, ainda é possível usar a palavra reservada **`and`** para condições adicionais dentro de um guard:

    .mixin (@a) when (isnumber(@a)) and (@a > 0) { ... }

E a palavra reservada **`not`** para negar as condições:

    .mixin (@b) when not (@b > 0) { ... }

Regras aninhadas
----------------

LESS lhe dá a habilidade de usar *aninhamento*, com ou sem cascateamento (cascading).
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

Em LESS, nós também podemos escrever da seguinte maneira:

    #header {
      color: black;

      .navigation {
        font-size: 12px;
      }
      .logo {
        width: 300px;
        &:hover { text-decoration: none }
      }
    }

Ou desta outra maneira:

    #header        { color: black;
      .navigation  { font-size: 12px }
      .logo        { width: 300px;
        &:hover    { text-decoration: none }
      }
    }

O código resultante é mais conciso, e mimica a estrutura da sua `árvore DOM`.

Perceba o combinador `&`--ele é usado quando queremos usar um seletor aninhado concatenado aos seu seletor pai, ao contrário de atuar como um descendente.
Isto é muito importante para pseudo-classe como `:hover` e `:focus`.

Por exemplo:

    .bordered {
      &.float {
        float: left;
      }
      .top {
        margin: 5px;
      }
    }

Vai gerar a saída:

    .bordered.float {
      float: left;
    }
    .bordered .top {
      margin: 5px;
    }

Operações
----------

Qualquer número, cor ou variável pode ser usado em operações. Alguns exemplos:

    @base: 5%;
    @filler: @base * 2;
    @other: @base + @filler;

    color: #888 / 4;
    background-color: @base-color + #111;
    height: 100% / 2 + @filler;

A saída é basicamente o que se espera-LESS entende a diferença entre cores e unidade. Se uma unidade é usada em uma operação, como em:

    @var: 1px + 5;

LESS irá usar esta unidade para a saída final-`6px` neste caso.

Parênteses também são permitidos nessas operações:

    width: (@var + 5) * 2;

E são obrigatórios em valores compostos:

    border: (@width * 2) solid black;

Funcões de cores
----------------

LESS fornece uma variedade de funções que transformam cores. Cores são primeiramente convertidas para
o formato *HSL*, e então manipuladas a nível de canal:

    lighten(@color, 10%);     // return a color which is 10% *lighter* than @color
    darken(@color, 10%);      // return a color which is 10% *darker* than @color

    saturate(@color, 10%);    // return a color 10% *more* saturated than @color
    desaturate(@color, 10%);  // return a color 10% *less* saturated than @color

    fadein(@color, 10%);      // return a color 10% *less* transparent than @color
    fadeout(@color, 10%);     // return a color 10% *more* transparent than @color
    fade(@color, 50%);        // return @color with 50% transparency

    spin(@color, 10);         // return a color with a 10 degree larger in hue than @color
    spin(@color, -10);        // return a color with a 10 degree smaller hue than @color

    mix(@color1, @color2);    // return a mix of @color1 and @color2

O uso das funções é bem simples:

    @base: #f04615;

    .class {
      color: saturate(@base, 5%);
      background-color: lighten(spin(@base, 8), 25%);
    }

Também é possível extrair informaçã das cores:

    hue(@color);        // returns the `hue` channel of @color
    saturation(@color); // returns the `saturation` channel of @color
    lightness(@color);  // returns the 'lightness' channel of @color
    alpha(@color);      // returns the 'alpha' channel of @color

Isto é útil se quisermos criar uma nova cor baseada em um canal de uma outra cor, por exemplo:

    @new: hsl(hue(@old), 45%, 90%);

`@new` vai ter o *hue* de `@old`, e sua própria saturation e lightness.

Funções Matemáticas
-------------------

LESS fornece algumas funções matemáticas bastante úteis que podemos usar com valores:

    round(1.67); // returns `2`
    ceil(2.4);   // returns `3`
    floor(2.6);  // returns `2`

Se precisarmos transformar um valor em uma porcentagem, poder fazer isso com a função `percentage`:

    percentage(0.5); // returns `50%`

Namespaces
----------

Algumas vezes, queremos agrupar nossas variáveis e mixins, por motivos de organização, ou apenas para oferecer encapsulamento.
Podemos fazer isso de forma intuitiva em LESS-digamos que queremos criar um pacote com alguns mixins e variáveis com o nome `#bundle`, para re-uso posterior ou para distribuição:

    #bundle {
      .button () {
        display: block;
        border: 1px solid black;
        background-color: grey;
        &:hover { background-color: white }
      }
      .tab { ... }
      .citation { ... }
    }

Agora se quisermos adicionar o classe `.button` do mixin ao nosso `#header a`, podemos fazer da seguinte forma:

    #header a {
      color: orange;
      #bundle > .button;
    }

Escopo
------

Escopo em LESS é muito semelhante ao de linguagens de programação. Variáveis e mixins são primeiro procurados localmente, e caso não sejam encontrados, o compilador irá procurar no escopo pai, e assim por diante.

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

Comentários
-----------

Comentários seguindo o padrão CSS são preservados em LESS:

    /* Hello, I'm a CSS-style comment */
    .class { color: black }

Comentário de uma linha também são válidos no LESS, mas são 'silenciosos',
eles não aparecem no CSS compilado:

    // Hi, I'm a silent comment, I won't show up in your CSS
    .class { color: white }

Importação
----------

Podemos importar arquivos `.less`, e todas as variáveis e mixins nestes estarão disponíveis no arquivo principal.
A extensão `.less` é opcional, então ambas as regras são válidas:

    @import "lib.less";
    @import "lib";

Se quisermos importar um arquivo CSS, e não quisermos que LESS processe ele, apenas usamos a extensão `.css`:

    @import "lib.css";
A diretiva irá ficar como está, e acabará como uma saída CSS.

Interpolação de strings
-----------------------

Variáveis podem ser incluídas dentro de strings de uma maneira similar a ruby ou PHP, com o construtor `@{name}`

    @base-url: "http://assets.fnord.com";
    background-image: url("@{base-url}/images/bg.png");

Escaping
--------

Podemos precisar usar um valor CSS na saída que não é válido pela sintaxe do CSS,
ou utiliza uma sintaxe proprietária que LESS não reconhece.

Para gerar a saída de tal valor, o colocamos dentro de uma string começando com `~`, por exemplo:

    .class {
      filter: ~"ms:alwaysHasItsOwnSyntax.For.Stuff()";
    }

Isto é chamado de "escaped valur", que irá resultar em:

    .class {
      filter: ms:alwaysHasItsOwnSyntax.For.Stuff();
    }

Avaliação (evaluation) de JavaScript
------------------------------------

Expressões JavaScript podem ser avaliadas (evaluated) como valores detro dos arquivos .less.
Isto é feito colocando a expressões entre crases:

    @var: `"hello".toUpperCase() + '!'`;

Gera a saída:

    @var: "HELLO!";

Também é possível usar interpolação e escaping com as strings:

    @str: "hello";
    @var: ~`"@{str}".toUpperCase() + '!'`;

Gera a saída:

    @var: HELLO!;

É possível inclusive acessar o ambiente JavaScript:

    @height: `document.body.clientHeight`;

Se quisermos dar um parse em uma string JavaScript como uma cor hexadecimal, podemos utilizar a função `color`:

    @color: color(`window.colors.baseColor`);
    @darkcolor: darken(@color, 10%);
