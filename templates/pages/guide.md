Uso no lado do cliente
=================

Vincule seus estilos `.less` com o atributo `rel` definido como '`stylesheet/less`':

    <link rel="stylesheet/less" type="text/css" href="styles.less">

Depois baixe o `less.js` no topo desta p&aacute;gina, e o inclua no elemento `<head>` da sua p&aacute;gina, desta forma:

    <script src="less.js" type="text/javascript"></script>

Certifique-se de incluir suas folhas estilos *antes* do script.

Modo watch
----------

*Modo watch* &eacute; uma caracter&iacute;stica do lado do cliente que permite que seus estilos sejam automaticamente atualizados assim que forem alterados.

Para ativ&aacute;-la, acrescente "`#!watch`" na URL do navegador e depois atualize a p&aacute;gina. Alternativamente, voc&ecirc; pode rodar `less.watch()` diretamente do console.

Uso no lado do servidor
=================

Instala&ccedil;&atilde;o
------------

A maneira mais f&aacute;cil de instalar o LESS no servidor, &eacute; por [npm](http://github.com/isaacs/npm), o gerenciador de pacotes node, desta forma:

    $ npm install less

Uso
---

Uma vez instalado, voc&ecirc; pode chamar o compilador pelo node, assim:

    var less = require("less");
    
    less.render(".class { width: 1 + 1 }", function (e, css) {
        console.log(css);
    });

que produzir&aacute; a sa&iacute;da

    .class {
      width: 2;
    }

voc&ecirc; tamb&eacute;m pode chamar o analisador e o compilador manualmente:

    var parser = new(less.Parser);

    parser.parse(".class { width: 1 + 1 }", function (err, tree) {
        if (err) { return console.error(err) }
        console.log(tree.toCSS());
    });

Configura&ccedil;&atilde;o
-------------

Voc&ecirc; pode especificar algumas op&ccedil;&otilde;es para o compilador:

    var parser = new(less.Parser)({
        paths: [".", "./lib"], // Especifique caminhos de busca para as diretivas @import
        filename: "style.less" // Especifique um nome de arquivo, para mensagens de erros melhores
    });

    parser.parse(".class { width: 1 + 1 }", function (e, tree) {
        tree.toCSS({ compress: true }); // Minificar (minify) a sa&iacute;da
    });

Uso na linha de comando
------------------

Less possui um bin&aacute;rio, que permite que voc&ecirc; chame o compilador diretamente da linha de comando, assim:

    $ lessc styles.less

Isto produzir&aacute; como sa&iacute;da o CSS compilado para `stdout`, voc&ecirc; pode ent&atilde;o redirecion&aacute;-lo para um arquivo a sua escolha:

    $ lessc styles.less > styles.css

Para produzir o CSS minificado (minified), apenas passe a op&ccedil;&atilde;o `-x`. Se voc&ecirc; quiser se envolver mais com o processo de minifica&ccedil;&atilde;o,
o [YUI CSS Compressor](http://developer.yahoo.com/yui/compressor/css.html) tamb&eacute;m est&aacute; dispon&iacute;vel atrav&eacute;s da op&ccedil;&atilde;o `--yui-compress`.

