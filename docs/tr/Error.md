
## Error Katmanı

Error katmanı <a href="https://github.com/zendframework/zend-stratigility" target="_blank">Http/Zend/Stratigility</a> paketi ile birlikte gelir. Bu katman uygulamanızda gerçekleşecek istisnai hataları yada bir katman içerisinde gerçekleşebilecek özel hataları kontrol eder. Eğer bir katman içerisinden <kbd>$err = ""</kbd> değişkeni ile bir hata gönderilirse uygulama akışı kesilir ve katman hata çıktısı ile birlikte response nesnesine geri döner. Hata olmayan durumlarda ise bir sonraki $next() fonksiyonu çalışır ve uygulama normal akışına devam eder.


```php
$err = 'example error !';

return $next($request, $response, $err);
```

#### Konfigürasyon 

Error katmanının <kbd>app/middlewares.php</kbd> içerisine eklenmesi gerekmez uygulama tarafından katmanlar içerisine kendiliğinden dahil edilir. 

#### Kurulum

```php
http://github.com/obullo/http-middlewares/
```

Yukarıdaki kaynaktan <kbd>Error.php</kbd> dosyasını uygulamanızın <kbd>app/classes/Http/Middlewares/</kbd> klasörüne kopyalayın.

#### Katman Hataları

Bir katman içerisinde hatalar oluşturmak için aşağıdaki gibi <kbd>$err</kbd> değişkenini kullanabilirsiniz.

```php
public function __invoke(Request $request, Response $response, callable $next = null)
{
    $err = 'test error !';

    return $next($request, $response, $err);
}
```

Yukarıdaki örneği uyguladığınızda Error katmanı çalıştırılır ve <kbd>test error !</kbd> hatası ile karşılaşırsınız.

#### İstisnai Hatalar

Uygulama içerisinde aşağıdaki gibi bir istisnai hata oluştuğunda hata Error katmanına gönderilir.

```php
use Obullo\Http\Controller;

class Welcome extends Controller
{
    public function index()
    {
        throw new \Exception("Im a test exception error !");

        $this->view->load('welcome');
    }
}
```
#### Hata Yönetimi

Error katmanından sadece uygulama içerisindeki istisnai hatalar ve <kbd>$err</kbd> değişkeni ile gönderilen hatalar kontrol edilebilir. Uygulama evrensel hataları ise <kbd>app/errors.php</kbd> dosyasından yönetilir. Error katmanı içerisinden <kbd>$this->c['app']->exceptionError();</kbd> metodu kullanılarak istisnai hatalar <kbd>app/errors.php</kbd> dosyasına yönlendirilmiş olur.

```php
class Error implements ErrorMiddlewareInterface, ContainerAwareInterface
{
    public function __invoke($error, Request $request, Response $response, callable $out = null)
    {
        if (is_string($error)) {

            echo $error;
        }
        if (is_object($error)) {
            
            if ($this->c['app']->env() == 'local') {

                $exception = new \Obullo\Error\Exception;
                echo $exception->make($error);

                $this->c['app']->exceptionError($error);  // Forward exceptions to app/errors.php

            } else {
            
                echo $error->getMessage();

                $this->c['app']->exceptionError($error);
            }
        }
        return $response;
    }
}
```

Yukarıdaki örnekte istisnai hataları ekrana dökmek için  <kbd>$exception->make($error)</kbd> metodu kullanılıyor.