
## RewriteLocale Katmanı

Bu katman uygulamaya <kbd>http://example.com/welcome</kbd> olarak gelen istekleri mevcut yerel dili ekleyerek <kbd>http://example.com/en/welcome</kbd> adresine yönlendirir.

#### Konfigürasyon

Eğer tanımlı değilse <kbd>app/middlewares.php</kbd> dosyası içerisine bu katmanını tanımlayın.

```php
$middleware->register(
    [
        'RewriteLocale' => 'Http\Middlewares\RewriteLocale',
    ]
);
```

#### Kurulum

```php
http://github.com/obullo/http-middlewares/
```

Yukarıdaki kaynaktan <kbd>RewriteLocale.php</kbd> dosyasını uygulamanızın <kbd>app/classes/Http/Middlewares/</kbd> klasörüne kopyalayın.

#### Çalıştırma

Aşağıdaki örnek uygulamanıza gelen tüm istekler için RewriteLocale katmanını çalıştırır.

```php
$router->group(
    [
        'middleware' => array('RewriteLocale')
    ],
    function () {

        $this->get('(?:en|de|es|tr)', 'welcome');     // example.com/en
        $this->get('(?:en|de|es|tr)(/)', 'welcome');  // example.com/en/
        $this->get('(?:en|de|es|tr)/(.*)', '$1');     // example.com/en/examples/helloWorld

        $this->attach('.*');
    }
);
```

Sadece belirli bir <kbd>domain</kbd> adresi yada <kbd>klasörler</kbd> için katmanın çalışmasını sınırlandırabilirsiniz.

```php
$router->group(
    [
        'domain' => '^example.com$',
        'middleware' => array('RewriteLocale')
    ],
    function () {

        $this->get('(?:en|de|es|tr)', 'welcome');     // example.com/en
        $this->get('(?:en|de|es|tr)(/)', 'welcome');  // example.com/en/
        $this->get('(?:en|de|es|tr)/(.*)', '$1');     // example.com/en/examples/helloWorld

        $this->attach('/');
        $this->attach('welcome');
        $this->attach('sports/.*');
        $this->attach('support/.*');
    }
);
```
