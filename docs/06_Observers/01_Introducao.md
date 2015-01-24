# Introdução aos Observers

Como qualquer bom sistema orientado a objetos, Magento implementa um padrão de evento/observador (ou event/observer).

Uma explicação bem informal, mas que fica simples de se entender sobre eventos é a seguinte. Você irá ter um evento toda vez que disser a palavra *quando* ou *toda vez que*.

Exemplos:
    * Eu quero sempre receber um email [QUANDO | TODA VEZ QUE] o usuário se cadastrar no site.
    * Eu quero inserir o nome do usuário e o id do produto em uma determinada tabela [QUANDO | TODA VEZ QUE] o usuário removê-lo do carrinho.
    * Eu quero que o usuário seja alertado [QUANDO | TODA VEZ QUE] o seu comentário for aprovado.
    * Eu quero que [QUANDO | TODA VEZ QUE] um usuário faça login, seu email seja inserido em um log txt.

Resumindo. Quando você quiser que uma ação aconteça sempre que outra ação acontecer, você estará precisando de um OBSERVER.

Como determinadas ações acontecem durante uma solicitação de página (a modelo é salvo, um usuário faz login, etc.), Magento irá emitir um sinal de evento.
Ao criar seus próprios módulos, você pode "ouvir" (caso já tenha ouvido a expressão criar um listener) esses eventose fazer suas ações.

## Primeiro exemplo (Estamos de olho!)

Digamos que você queria receber um e-mail toda vez que um determinado cliente fizer seu login.
Você pode ouvir o evento "customer_login" (instalação em config.xml).

Vamos fazer um simples log dos usuários que acessaram a loja e salvar em um arquivo txt e emitir um alerta escrito "Estamos de olho".

Em seu arquivo `config.xml` em `/etc` insira na tag `globals`.

```xml
<events>
    <customer_login>
        <observers>
            <unique_name>
                <type>singleton</type>
                <class>meutema/observer</class>
                <method>estamosDeOlho</method>
            </unique_name>
        </observers>
    </customer_login>
</events>
```

Agora uma observação, diferente dos Helpers que possuem uma pasta própria, os observers não às possui. Os mesmos ficam na pasta Model.
Portanto, insira na pasta Model o arquivo `Observer.php`.

```php
<?php

class Biblioteca_MeuTema_Model_Observer
{
    public function estamosDeOlho(Varien_Event_Observer $observer)
    {
        $email = $observer->getData()['customer']->getEmail();
        $horario = date('H:i:s');
        $dia = date('d/m/Y');

        $file = fopen(__DIR__ . '/emails.txt', 'a+');
        fwrite($file, "O usuário {$email} se conectou às {$horario} no dia {$dia}.\n");
        fclose($file);

        // Chamando o flash message
        Mage::getSingleton('core/session')->addWarning('Estamos de olho!');
    }
}

```

Crie o arquivo emails.txt na pasta Model e deixe-o com a permissão 0777.
Faça login e veja a magia acontecer.

![estamos de olho](/img/estamos-de-olho.png "Estamos de olho")