Refer&ecirc;ncia
==========

Teste
-----------

    vows [FILE, ...] [options]

### Rodando testes #

    $ vows test-1.js test-2.js
    $ vows tests/*

Modo watch

    $ vows -w
    $ vows --watch

### Op&ccedil;&otilde;es #

<table cellspacing="10">
  <tr>
    <td><code>-v</code>, <code>--verbose</code></td>
    <td>Modo Verboso</td>
  </tr>
  <tr>
    <td><code>-w</code>, <code>--watch</code></td>
    <td>Modo watch</td>
  </tr>
  <tr>
    <td><code>-m STRING</code></td>
    <td>Correspond&ecirc;ncia de string: Apenas roda testes com <code>STRING</code> em seu t&iacute;tulo</td>
  </tr>
  <tr>
    <td><code>-r REGEXP</code></td>
    <td>Correspond&ecirc;ncia de regexp: Apenas roda testes com <code>REGEXP</code> em seu t&iacute;tulo</td>
  </tr>
  <tr>
    <td><code>--json</code></td>
    <td>Usa o JSON reporter</td>
  </tr>
  <tr>
    <td><code>--spec</code></td>
    <td>Usa o Spec reporter</td>
  </tr>
  <tr>
    <td><code>--dot-matrix</code></td>
    <td>Usa o Dot-Matrix reporter</td>
  </tr>
  <!-- <tr> -->
  <!--   <td><code>-no-color</code></td> -->
  <!--   <td>Don't use terminal colors</td> -->
  <!-- </tr> -->
  <tr>
    <td><code>--version</code></td>
    <td>Mostra a vers&atilde;o</td>
  </tr>
  <tr>
    <td><code>-s</code>, <code>--silent</code></td>
    <td>N&atilde; reporta</td>
  </tr>
  <tr>
    <td><code>--help</code></td>
    <td>Mostra ajuda</td>
  </tr>
</table>

Fun&ccedil;&otilde;es de declara&ccedil;&atilde;o
-------------------

### igualdade #

    assert.equal          (4, 4);
    assert.strictEqual    (4 > 2, true);

    assert.notEqual       (4, 2);
    assert.strictNotEqual (1, true);

### tipo #

    assert.isFunction (function () {});
    assert.isObject   ({goo:true});
    assert.isString   ('goo');
    assert.isArray    ([4, 2]);
    assert.isNumber   (42);
    assert.isBoolean  (true);

    assert.typeOf     (42, 'number');
    assert.instanceOf ([], Array);

### booleano #

    assert.isTrue  (true);
    assert.isFalse (false);

### null, undefined, NaN #

    assert.isNull      (null);
    assert.isNotNull   (undefined);

    assert.isUndefined ('goo'[9]);
    assert.isNaN       (0/0);

### inclus&atilde;o #

    assert.include ([4, 2, 0], 2);
    assert.include ({goo:true}, 'goo');
    assert.include ('goo', 'o');

### correspond&ecirc;ncia regexp #

    assert.match ('hello', /^[a-z]+/);

### tamanho #

    assert.length ([4, 2, 0], 3);
    assert.length ('goo', 3);

### vazio #

    assert.isEmpty ([]);
    assert.isEmpty ({});
    assert.isEmpty ("");

### exce&ccedil;&atilde;o #

- assert.throws
- assert.doesNotThrow


