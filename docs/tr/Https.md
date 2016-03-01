
## Https Katmanı

Uygulamanızda güvenli protokol ile başlaması gereken <kbd>http://</kbd> isteklerini <kbd>https://</kbd> protokolüne yönlendirir.

#### Konfigürasyon

Eğer tanımlı değilse <kbd>app/middlewares.php</kbd> dosyası içerisine Https katmanını tanımlayın.

```php
$middleware->register(
    [
        'Https' => 'Http\Middlewares\Https',
    ]
);
```

#### Kurulum

```php
http://github.com/obullo/http-middlewares/
```

Yukarıdaki kaynaktan <kbd>Https.php</kbd> dosyasını uygulamanızın <kbd>app/classes/Http/Middlewares/</kbd> klasörüne kopyalayın.

#### Çalıştırma

Aşağıdaki örnekde <kbd>http://examples.com/hello</kbd> adresi için tanımlı bir route kuralı gösteriliyor.

```php
$router->get('hello/*', 'welcome/index')->middleware('Https');
```

Eğer uygulamanıza <kbd>http://examples.com/hello</kbd> isteği gelirse, istek güvenli adrese yani <kbd>https://examples.com/hello</kbd> adresine yönlendirilir. Eğer birden fazla güvenli adresiniz varsa onları aşağıdaki gibi bir grup içinde tanımlamak daha doğru olacaktır.

```php
$router->group(
    [
    	'name' => 'Secure',
    	'middleware' => array('Https')
    ],
    function () {

        $this->get('orders/pay');
        $this->get('orders/bank_transfer');
        
        $this->attach('.*');
    }
);
```
