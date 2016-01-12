
## Https Katmanı

> Uygulamada belirli adreslere gelen <b>http://</b> isteklerini <b>https://</b> protokolüne yönlendirir.

#### Kurulum

```php
http://github.com/obullo/http-middlewares/
```

Yukarıdaki kaynaktan <kbd>Https.php</kbd> dosyasını uygulamanızın <kbd>app/classes/Http/Middlewares/</kbd> klasörüne kopyalayın.

#### Çalıştırma

Aşağıdaki örnek tek bir route için https katmanı tayin etmenizi sağlar.

```php
$c['router']->get('hello$', 'welcome/index')->middleware('Https');
```

Eğer birden fazla güvenli adresiniz varsa onları aşağıdaki gibi bir grup içinde tanımlamak daha doğru olacaktır.

```php
$c['router']->group(
    ['name' => 'Secure', 'domain' => 'framework', 'middleware' => array('Https')],
    function () {

        $this->get('orders/pay');
        $this->get('orders/bank_transfer');
        
        $this->attach('.*');
    }
);
```