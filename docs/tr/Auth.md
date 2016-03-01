
### Auth Katmanı

Başarılı oturum açmış ( yetkinlendirilmiş ) kullanıcılara ait katmandır. Genellikle route yapısında Guest katmanı ile birlikte kullanılır.

<a name="auth-configuration"></a>

#### Konfigürasyon

Eğer tanımlı değilse <kbd>app/middlewares.php</kbd> dosyası içerisine Auth ve Guest katmanlarını tanımlayın.

```php
$middleware->register(
    [
        'Auth' => 'Http\Middlewares\Auth',
        'Guest' => 'Http\Middlewares\Guest',
    ]
);
```

#### Kurulum

```php
http://github.com/obullo/http-middlewares/
```

Yukarıdaki kaynaktan <kbd>Auth.php</kbd> dosyasını uygulamanızın <kbd>app/classes/Http/Middlewares/</kbd> klasörüne kopyalayın. 

### Çalıştırma

Uygulamanıza giriş yapmış kullanıcılara ait bir katman oluşması için belirli bir route grubu yaratıp Auth katmanını <kbd>app/routes.php</kbd> içerisine aşağıdaki gibi ekleyin. Route grubu içerisinde <b>$this->attach()</b> metodunu kullanarak yetkili kullanıcılara ait sayfayı yada sayfaları belirleyin.

```php
$router->group(
    [
        'name' => 'AuthorizedUsers',
        'middleware' => array('Auth', 'Guest')
    ],
    function () {

        $this->attach('membership/restricted');
    }
);
```

Eğer bu katmanları bir klasör için kullanıyorsanız attach metodu içerisinde düzenli ifade kullanabilirsiniz.

```php
$router->group(
    [
        'name' => 'AuthorizedUsers',
        'domain' => 'mydomain.com', 
        'middleware' => array('Auth','Guest')
    ],
    function () {

        $this->attach('accounts/.*');
    }
);
```

Yukarıdaki örnekte <b>folders/accounts</b> klasörü içerisindeki tüm sayfalarda <b>Auth</b> ve <b>Guest</b> katmanları çalışır.

### Tekil Oturum Açma Özelliği

Tekil oturum açma özelliği opsiyonel olarak kullanılır. Auth katmanı içerisinde bu özellik çağrıldığında birden fazla aygıtta yada birbirinden farklı tarayıcılarda oturum açıldığında açılan tüm önceki oturumlar sonlanır ve en son açılan oturum aktif kalır. Unique.session özelliği <kbd>providers/user.php</kbd> konfigürasyon dosyasından kapatılıp açılabilir.

```php

return array(

    'middleware' => [
        'unique.session' => true
    ]
);
```

Tekil oturum açma özelliğinin çalışabilmesi için Auth katmanı içerisinde <kbd>$this->killSessions()</kbd> metodunun aşağıdaki gibi kullanılıyor olması gerekir.

```php
public function __invoke(Request $request, Response $response, callable $next = null)
{
    if ($this->getContainer()->get('user')->identity->check()) {

        $this->killSessions();  // Terminate multiple logins

    }
    $err = null;

    return $next($request, $response, $err);
}
```
