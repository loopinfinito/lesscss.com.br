Uso no lado do cliente
=================

Vincule seus estilos `.less` com o atributo `rel` definido como "`stylesheet/less`":

    <link rel="stylesheet/less" type="text/css" href="styles.less">

Depois baixe o `less.js` no topo desta página, e o inclua no elemento `<head>` da sua página, desta forma:

    <script src="less.js" type="text/javascript"></script>

Certifique-se de incluir suas folhas estilos *antes* do script.

Modo assistido
----------

*Modo assistido* é uma característica do lado do cliente que permite que seus estilos sejam automaticamente atualizados assim que forem alterados.

Para ativá-la, acrescente '`#!watch`' na URL do navegador e depois atualize a página. Alternativamente, você pode rodar `less.watch()` diretamente do console.

Uso no lado do servidor
=================

Instalação
------------

A maneira mais fácil de instalar o LESS no servidor, é por [npm](http://github.com/isaacs/npm), o gerenciador de pacotes node, desta forma:

    $ npm install less

Uso
---

Uma vez instalado, você pode chamar o compilador pelo node, assim:

    var less = require('less');
    
    less.render('.class { width: 1 + 1 }', function (e, css) {
        console.log(css);
    });

que produzirá a saída

    .class {
      width: 2;
    }

você também pode chamar o analisador e o compilador manualmente:

    var parser = new(less.Parser);

    parser.parse('.class { width: 1 + 1 }', function (err, tree) {
        if (err) { return console.error(err) }
        console.log(tree.toCSS());
    });

Configuração
-------------

Você pode especificar algumas opções para o compilador:

    var parser = new(less.Parser)({
        paths: ['.', './lib'], // Especifique caminhos de busca para as diretivas @import
        filename: 'style.less' // Especifique um nome de arquivo, para mensagens de erros melhores
    });

    parser.parse('.class { width: 1 + 1 }', function (e, tree) {
        tree.toCSS({ compress: true }); // Minificar (minify) a saída
    });

Uso na linha de comando
------------------

Less possui um binário, que permite que você chame o compilador diretamente da linha de comando, assim:

    $ lessc styles.less

Isto produzirá como saída o CSS compilado para `stdout`, você pode então redirecioná-lo para um arquivo a sua escolha:

    $ lessc styles.less > styles.css

Para produzir o CSS minificado (minified), apenas passe a opção `-x`. Se você quiser se envolver mais com o processo de minificação,
o [YUI CSS Compressor](http://developer.yahoo.com/yui/compressor/css.html) também está disponível através da opção `--yui-compress`.

