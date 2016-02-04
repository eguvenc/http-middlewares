
### Guest Katmanı

Oturum açmamış ( yetkinlendirilmemiş ) kullanıcılara ait bir katman oluşturur. Bu katman auth paketini çağırarak kullanıcının sisteme yetkisi olup olmadığını kontrol eder ve yetkisi olmayan kullanıcıları sistem dışına yönlendirir. Route yapısında Auth katmanı ile birlikte kullanılması önerilir.
<a name="auth-configuration"></a>

#### Konfigürasyon

Eğer tanımlı değilse <kbd>app/middlewares.php</kbd> dosyası içerisine Guest katmanını tanımlayın.

```php
$middleware->add(
    [
        'Auth',
        'Guest'
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
$router->group(
    [
        'name' => 'AuthorizedUsers',
        'middleware' => array('Guest')
    ],
    function () {

        $this->attach('membership/restricted');
    }
);
```
Eğer bu katmanı bir modül için kullanmak istiyorsanız attach metodu içerisinde düzenli ifade kullanabilirsiniz.

```php
$router->group(
    [
        'name' => 'UnAuthorizedUsers',
        'domain' => 'mydomain.com', 
        'middleware' => array('Guest')
    ],
    function () {

        $this->attach('accounts/.*');
    }
);
```
