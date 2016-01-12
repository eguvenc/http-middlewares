
### Guest Katmanı

> Oturum açmamış ( yetkinlendirilmemiş ) kullanıcılara ait bir katman oluşturur. Bu katman auth paketini çağırarak kullanıcının sisteme yetkisi olup olmadığını kontrol eder ve yetkisi olmayan kullanıcıları sistem dışına yönlendirir. Route yapısında Auth katmanı ile birlikte kullanılır.
<a name="auth-configuration"></a>

#### Kurulum

```php
http://github.com/obullo/http-middlewares/
```

Yukarıdaki kaynaktan <kbd>Guest.php</kbd> dosyasını uygulamanızın <kbd>app/classes/Http/Middlewares/</kbd> klasörüne kopyalayın.


#### Konfigürasyon

Eğer tanımlı değilse <kbd>app/middlewares.php</kbd> dosyası içerisine Guest katmanını tanımlayın.

```php
$c['middleware']->register(
    [
        .
        .
        'Auth' => 'Http\Middlewares\Auth',
        'Guest' => 'Http\Middlewares\Guest',
    ]
);
```

Guest katmanına bir örnek.


```php
public function __invoke(Request $request, Response $response, callable $next = null)
{
    if ($this->user->identity->guest()) {

        $this->c['flash']->info('Your session has been expired.');

        return $response->redirect('/examples/membership/login/index');
    }
    $err = null;

    return $next($request, $response, $err);
}
```