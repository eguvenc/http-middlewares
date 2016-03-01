
### Guest Katmanı

Oturum açmamış ( yetkinlendirilmemiş ) kullanıcılara ait bir katman oluşturur. Bu katman auth paketini çağırarak kullanıcının sisteme yetkisi olup olmadığını kontrol eder ve yetkisi olmayan kullanıcıları sistem dışına yönlendirir. Route yapısında Auth katmanı ile birlikte kullanılması önerilir.
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

Yukarıdaki kaynaktan <kbd>Guest.php</kbd> dosyasını uygulamanızın <kbd>app/classes/Http/Middlewares/</kbd> klasörüne kopyalayın. 

#### Çalıştırma

Katmanın çalışması için route yapısına tutturulması gerekir.

```php
/**
* Unauthorized Users
*/
$router->group(
    [
        'middleware' => array('Guest')
    ],
    function () {

        $this->attach('membership/restricted');
    }
);
```
Eğer bu katmanı bir <kbd>klasör</kbd> için kullanmak istiyorsanız <kbd>attach</kbd> metodu içerisinde düzenli ifade kullanabilirsiniz.

```php
/**
* Unauthorized Users
*/
$router->group(
    [
        'middleware' => array('Guest')
    ],
    function () {

        $this->attach('accounts/.*');
    }
);
```
