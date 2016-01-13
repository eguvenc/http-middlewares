
## RewriteLocale Katmanı

Bu katman uygulamaya <b>http://example.com/welcome</b> olarak gelen istekleri mevcut yerel dili ekleyerek <b>http://example.com/en/welcome</b> adresine yönlendirir.

#### Konfigürasyon

Eğer tanımlı değilse <kbd>app/middlewares.php</kbd> dosyası içerisine bu katmanını tanımlayın.

```php
$c['middleware']->register(
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

Aşağıdaki örnek genel ziyaretçiler route grubu için RewriteLocale katmanını çalıştırır.

```php
$c['router']->group(
    [
        'name' => 'Locale', 
        'domain' => 'mydomain.com', 
        'middleware' => array('RewriteLocale')
    ],
    function () {

        $this->get('(?:en|tr|de|nl)/(.*)', '$1');
        $this->get('(?:en|tr|de|nl)', 'welcome');

        $this->attach('.*');
    }
);
```

Sadece belirli dizinler için katmanın çalışması sınırlandırılabilir.

```php
$c['router']->group(
    [
        'name' => 'Locale',
        'domain' => '^example.com$',
        'middleware' => array('RewriteLocale')
    ],
    function () {

        $this->get('(?:en|tr|de|nl)/(.*)', '$1');
        $this->get('(?:en|tr|de|nl)', 'welcome/index');

        $this->attach('/');
        $this->attach('welcome');
        $this->attach('sports/.*');
        $this->attach('support/.*');
    }
);
```