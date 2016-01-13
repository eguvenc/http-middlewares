
## Yetki Katmanları

Auth katmanı uygulamaya giriş yapmış olan kullanıcıları kontrol ederken Guest katmanı ise uygulamaya giriş yetkisi olmayan kullanıcıları kontrol eder. Auth ve Guest katmanlarının çalışabilmesi için route yapınızda middleware anahtarına ilgili modül için birkez tutturulmaları gerekir.

#### Auth Katmanı

Başarılı oturum açmış ( yetkinlendirilmiş ) kullanıcılara ait katmandır. 

#### Guest Katmanı

Oturum açmamış ( yetkinlendirilmemiş ) kullanıcılara ait bir katman oluşturur. Bu katman auth paketini çağırarak kullanıcının sisteme yetkisi olup olmadığını kontrol eder ve yetkisi olmayan kullanıcıları sistem dışına yönlendirir. Route yapısında Auth katmanı ile birlikte kullanılır.

<a name="auth-configuration"></a>

#### Konfigürasyon

Eğer tanımlı değilse <kbd>app/middlewares.php</kbd> dosyası içerisine Auth ve Guest katmanlarını tanımlayın.

```php
$c['middleware']->register(
    [
        'Auth' => 'Http\Middlewares\Auth',
        'Guest' => 'Http\Middlewares\Guest',
    ]
);
```

### Çalıştırma

Uygulamanıza giriş yapmış kullanıcılara ait bir katman oluşması için belirli bir route grubu yaratıp Auth katmanını <kbd>app/routes.php</kbd> içerisine aşağıdaki gibi eklemeniz gerekir.
Son olarak route grubu içerisinde <b>$this->attach()</b> metodunu kullanarak yetkili kullanıcılara ait sayfayı yada sayfaları belirleyin.

```php
$c['router']->group(
    [
        'name' => 'AuthorizedUsers',
        'middleware' => array('Auth', 'Guest')
    ],
    function () {

        $this->attach('membership/restricted');
    }
);
```

Eğer bu katmanları bir modül için kullanıyorsanız attach metodu içerisinde düzenli ifade kullanabilirsiniz.

```php
$c['router']->group(
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

Yukarıdaki örnekte <b>modules/accounts</b> klasörü içerisindeki tüm sayfalarda <b>Auth</b> ve <b>Guest</b> katmanları çalışır.

### Tekil Oturum Açma Özelliği

Tekil oturum açma özelliği opsiyonel olarak kullanılır. Auth katmanı içerisinde bu özellik çağrıldığında birden fazla aygıtta yada birbirinden farklı tarayıcılarda oturum açıldığında açılan tüm önceki oturumlar sonlanır ve en son açılan oturum aktif kalır. Unique.session özelliği opsiyoneldir ve <kbd>service/user.php</kbd> konfigürasyon dosyasından kapatılıp açılabilir.

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
    if ($this->user->identity->check()) {

        $this->killSessions();  // Terminate multiple logins

    }
    $err = null;

    return $next($request, $response, $err);
}
```
