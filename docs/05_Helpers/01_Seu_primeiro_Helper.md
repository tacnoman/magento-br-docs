# Criando seu primeiro Helper

Helpers são classes que podem ser chamadas ou instanciadas na aplicação. Servem para ajudar, como o nome já diz.

Antes de tudo verifique em seu módulo se a pasta Helper existe, pois é nessa pasta que os helpers irão ficar.

Nesse exemplo iremos criar algo bem simples. Um helper chamado String que vai possuir um método estático chamado `toSlug()` que irá converter um texto para o formato de url amigável.
Crie na pasta Helper o arquivo String.php

```php
# app/code/[community|local]/[Namespace]/[Module]/Helper/String.php
# Vamos usar o namespace como Biblioteca e o Module como MeuTema

class Biblioteca_Module_MeuTema_Helper
{
    public function toSlug()
    {
        $text = preg_replace(
            ['/(à|á|ã|â)/','/(é|è|ẽ|ê)/','/(í|ì|ĩ|î)/','/(ó|ò|õ|ô)/','/(ú|ù|û)/','/ç/','/ /'],
            ['a','e','i','o','u','c','-'],
            strtolower($text)
        );

        $text = preg_replace('/([^a-z0-9-])/','',$text);
        $text = preg_replace('/(-)+/','-',$text);

        if(substr($text,-1) == '-') $text = substr($text,0,-1);
        if(substr($text,0,1) == '-') $text = substr($text,1);
        return $text;
    }
}

```

Nosso Helper já está pronto! Agora vamos ao nosso respectivo arquivo `config.xml` encontrado na pasta `/etc`.

```xml
<!-- .... -->
    <global>
        <helpers>
            <string_helper>
                <class>Biblioteca_MeuTema_Helper</class>
            </string_helper>
        </helpers>
    <!-- .... -->
    </global>
<!-- .... -->
```

Note que dentro da tag `helpers` criamos a tag `string_helper`. Essa tag pode ter o nome que desejar e será utilizada como parâmetro para resgatar o Helper que foi criado.
Observe também que a tag `class` não aponta para o Helper criado e sim para o namespace onde os Helpers estarão. Portanto ao criar outro Helper, não é necessário fazer mais alterações no xml.

Agora em sua view (ou controller) você já pode fazer a chamada ao Helper criado.

Veja um exemplo:

```php
    echo Mage::helper('string_helper/string')->toSlug('O rato roeu a roupa do rei de roma');
    
    // Output: o-rato-roeu-a-roupa-do-rei-de-roma
```

