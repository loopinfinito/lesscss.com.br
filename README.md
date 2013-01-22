A linguagem dinâmica de estilos.

LESS estende CSS com comportamento dinâmico como variáveis, mixins, operações e
funções. LESS roda tanto no lado do cliente (Chrome, Safari, Firefox) quanto no
lado do servidor, usando Node.js e Rhino.

Este repositório é o site da documentação em português.

## Como contribuir

Primeiro faça um clone do repositório
```bash
$ git clone git://github.com/loopinfinito/lesscss.com.br.git
```

Instale as _gem_
```bash
$ gem install toto
$ gem install thin
```

Agora rode localmente o site em sua máquina
```bash
$ cd lesscss.com.br
$ thin start -R config.ru
```

Todo o conteúdo da documentação está dentro da pasta `template/pages`. Os
arquivos `.md` estão formatados em MarkDown. Apenas modifique os arquivos,
verifique localmente e nos mande um _pull request_.
