
## Maintenance Katmanı

Bakıma alma eklentisi uygulamanın bütününü yada belirli alan adlarına yönelik bakıma alma özelliği sağlar. Eğer uygulamanıza ait birden fazla alan adı varsa bakıma alma özelliği bu adresler için de kullanılabilir.

#### Konfigürasyon

Eğer tanımlı değilse <kbd>app/middlewares.php</kbd> dosyası içerisine Maintenance katmanını tanımlayın.

```php
$middleware->register(
    [
        'Maintenance' => 'Http\Middlewares\Maintenance',
    ]
);
```

Katmanın çalışabilmesi için evrensel katmanlar içerisine eklenmesi gerekir.

```php
$middleware->init(
    [
        'Maintenance',
    ]
);
```

<a name="maintenance-add"></a>

#### Kurulum

```php
http://github.com/obullo/http-middlewares/
```

Yukarıdaki kaynaktan <kbd>Maintenance.php</kbd> dosyasını uygulamanızın <kbd>app/classes/Http/Middlewares/</kbd> klasörüne kopyalayın.

<a name="maintenance-run"></a>

#### Çalıştırma

Uygulamanızı bakıma almak için aşağıdaki komutu çalıştırın.

```php
php task app down
```

Uygulamanızı bakımdan çıkarmak için aşağıdaki komutu çalıştırın.

```php
php task app up
```

<a name="maintenance-configuration"></a>

#### Domain Konfigürasyonu

Eğer birden fazla alan adınız varsa ve alan adlarına yönelik bakıma alma yapmak istiyorsanız <kbd>app/$env/maintenance.php</kbd> dosyası içerisinden uygulamanızdaki domainlere ait düzenli ifadeleri belirleyin.

```php

return array(

    'root' => [
        'maintenance' => 'up',
    ],
    'mydomain' => [
        'maintenance' => 'up',
        'regex' => 'mydomain.com',
    ],
    'subdomain' => [
        'maintenance' => 'up',
        'regex' => 'sub.domain.com',
    ],
);
```

Dosya içerisindeki <kbd>maintenance</kbd> anahtarları domain adresinin bakıma alınıp alınmadığını kontrol eder, <kbd>regex</kbd> anahtarı ise geçerli domain adresleriyle eşleşme yapılabilmesine olanak sağlar. Domain adresinize uygun düzenli ifadeyi regex kısmına girin. Domain adresinizi route yapısına tutturmak <kbd>app/routes.php</kbd> dosyası içerisinde domain grubunuza ait <kbd>domain</kbd> ve <kbd>middleware</kbd> anahtarlarını aşağıdaki gibi güncelleyin.

```php
$router->group(
    [
        'domain' => 'sub.domain.com', 
        'middleware' => array('Maintenance')
    ],
    function () {

        $this->attach('(.*)');
    }
);
```

Şimdi test için bakım konfigürasyon dosyanızdaki ilgili domain adına adına ait <kbd>maintenance</kbd> değerini <kbd>down</kbd> olarak güncelleyin,

```php
php task app down subdomain
```

ve ardından alan adınızın bakıma alınıp alınmadığını sayfayı ziyaret ederek kontrol edin.

```php
http://sub.domain.com/
```

Herşey yolunda ise <kbd>resources/templates/maintenance.php</kbd> dosyası içerisinde bulunan <kbd>Service Unavailable</kbd> yazısı ile karşılaşmanız gerekir.
