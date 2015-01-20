# Blocos estáticos

Os blocos estáticos são blocos que possuem apenas conteúdo estático, podendo ser um texto ou um HTML.
Os blocos não possuem arquivos físicos. Eles são inseridos pelo painel administrativo.

Acesse o seu painel administrativo e vá em `CMS -> Static Blocks`.
Você irá encontrar uma listagem de blocos. Por padrão tem o `Footer Links` e o `Cookie restriction notice`.

## Código

Nos arquivos `.phtml` você pode inserir os blocos utilizando o seguinte código.

```php
<?php
    $block = $this->getLayout()->createBlock('cms/block');
    echo $block->setBlockId("footer_links")->toHtml();
?>
```

Na primeira linha criamos o bloco com a instância `cms/block`. Esse `cms/block` é uma instância da classe `Mage_Cms_Block_Block` localizada em `app/code/core/Mage/Cms/Block`.
Na segunta linha o método `setBlockId` você instancia qual bloco deve ser criado.

Volte ao painel em `CMS -> Static Blocks` e note a segunda coluna da tabela `Identifier`. Ela possui um slug com o id de cada bloco. Esse id é único e serve para o Magento saber qual bloco deve ser renderizado na tela.

Como os blocos fazem parte do Layout, é importante antes de chamar o método de criar o bloco chamar o layout.

O método `->toHtml()` converte processa o arquivo phtml para exibir na página.

## No CMS

Agora que você já entendeu como funciona as instâncias de blocos estáticos, você pode precisar em algum momento inserir um bloco estático dentro de outro pelo CMS do Magento.

A lógica é a mesma para o código listado acima, porém no CMS os códigos dinâmicos são inseridos entre chaves( `{{` ).

```
/**
 * {\{block type="cms/block" block_id="footer_links"}}
 */
 ```

 ## XML

 Para inserir pelo XML um bloco, basta adicionar o seguinte código

 ```xml
 <reference name="content">
   <block type="cms/block" name="block.name">
     <action method="setBlockId"><block_id>footer_links</block_id></action>
   </block>
 </reference>
 ```