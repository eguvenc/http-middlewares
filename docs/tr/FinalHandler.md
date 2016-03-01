
## Sonlandırıcı Katman

Final handler yani sonlandırıcı katman http katmanlarının sonuncusu olarak düşünülebilir. Uygulamada bir sonlandırıcı katman aşağıdaki işlemleri yürütebilir.

* Uygulama loglarını kapatmak
* Http başlıklarını göndermek
* Fatal error handler görevi üstlenmek
* Benchmark sürecini sonlandırmak
* <kbd>TerminableInterface</kbd> arayüzünü kullanan katmanların <kbd>terminate()</kbd> metodunu çalıştırmak
* Hataları yakalamak ve çevre ortamına göre farklı davranışlar sergilemek

Şu anki sürümde ise 2 adet katman sağlayıcısı destekleniyor. Bunlar aşağıda listelenmiştir.

* Zend Stratigility ( Varsayılan )
* <a href="https://github.com/obullo/relay-middleware" target="_blank">Relay</a> ( Opsiyonel )

Herbir katman sağlayıcının kendine ait sonlandırıcısı vardır.

#### Zend Katmanı Kurulumu

Zend sonlandırıcısı için index.php dosyasında aşağıdaki gibi <kbd>Http\Zend\Stratigility\MiddlewarePipe</kbd> sınıfı kullanılır.

```php
/*
|--------------------------------------------------------------------------
| Choose your middleware app
|--------------------------------------------------------------------------
*/
$app = new Obullo\Http\Zend\Stratigility\MiddlewarePipe($container);
/*
|--------------------------------------------------------------------------
| Create your http server
|--------------------------------------------------------------------------
*/
$server = Obullo\Http\Server::createServerFromRequest(
    $app,
    Obullo\Log\Benchmark::start($app->getRequest()),
    $app->getResponse()
);
/*
|--------------------------------------------------------------------------
| Run
|--------------------------------------------------------------------------
*/
$server->listen();
```

Zend middleware hakkında daha detaylı bilgi için <a href="https://github.com/zendframework/zend-stratigility" target="_blank">zend middleware</a> bağlantısını ziyaret edebilirsiniz.

#### Relay Katmanı Kurulumu

Relay katmanı kurulumu için <a href="https://github.com/obullo/relay-middleware" target="_blank">https://github.com/obullo/relay-middleware</a> adresini ziyaret edin.
