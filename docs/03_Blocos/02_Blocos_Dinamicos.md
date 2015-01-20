# Blocos Dinâmicos

Os blocos dinâmicos, diferente dos blocos estáticos, não são criados a partir do painel administrativo do Magento.
Como são dinâmicos, eles podem agir de maneiras diferentes de acordo com os dados que são enviados para ele.

O bloco consiste em um arquivo `.php` com uma classe que possui diversos métodos e um arquivo de template `.phtml` que tem acesso a esses métodos.

No seu tema corrente, crie a pasta `Block` (/app/code/(community ou local)/(namespace)/(tema)/Block) e dentro dela crie o arquivo `MeuBloco.php`.
Vamos usar como exemplo o namespace `Biblioteca` e como tema `MeuTema`.

```php
<?php

/**
 *  File: /app/code/community/Biblioteca/MeuTema/Block/MeuBloco.php
 */
class Biblioteca_MeuTema_Block_MeuBloco extends Mage_Core_Block_Template
{
    public function TesteDeMetodo()
    {
        return 'Retorno do método';
    }
}

```

Agora crie o arquivo de template:

```php
/**
 * File: /app/design/frontend/Biblioteca/MeuTema/default/template/MeuBloco/meubloco.phtml
 */

Sou um bloco.

<?php echo $this->TesteDeMetodo(); ?>

```

Como você pode perceber, a página de template pode acessar os métodos da classe `Biblioteca_MeuTema_Block_MeuBloco` pela variável `$this`.
Agora precisamos inserí-la no frontend.

No seu tema, você terá na pasta `/etc/` o arquivo config.xml. Abra-o e insira esse trecho de código xml.

```xml
<global>
    <blocks>
        <meubloco>
            <class>Biblioteca_MeuTema_Block</class>
        </meubloco>
    </blocks>
</global>
```

Algumas observações. Na tag `<global>`, como o próprio nome diz, você está inserindo informações que estarão disponíveis de forma global, ou seja, será para todo o Magento.
Na tag `<blocks>`, você está inserindo o bloco criado.

A tag em seguida `<meubloco>`, pode ter o nome um nome qualquer que lembre essa classe. Ele servirá para instanciar a classe ao template (para que você possua acesso aos métodos).
No último tutorial de blocos estáticos inserimos o type do block como `cms/page`, onde o `cms` é essa tag que indica a localizaçao dos blocos e page é o arquivo da classe `Page.php`.

No mesmo arquivo `.xml` você deve informar onde esse bloco dinâmico deve ficar no layout.
Insira o seguinte código.

```xml
<default>

    <reference name="right">
        <block type="meubloco/meubloco" name="right.olodum" template="MeuBloco/meubloco.phtml"></block>
    </reference>

</default>
```

Algumas observações são de extrema importância, começando pela tag `type`.
Note que inserimos `meubloco/meubloco`. O primeiro `meubloco` é a tag que criamos em `<global>` que é a refencia a classe e o segundo `meubloco` é o nome do arquivo que fica na pasta `Block`.

A tag reference, indica em qual parte do layout será inserido o template (nesse caso a coluna direita do layout).