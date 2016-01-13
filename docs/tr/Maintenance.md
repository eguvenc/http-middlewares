
## Maintenance Katmanı

Maintenance eklentisi uygulamanın bütününü yada belirli kısımlarını bakıma alma özelliği sunar. Eğer uygulamanıza ait birden fazla alan adı varsa bakıma alma özelliği bu adresler için de kullanılabilir.

#### Konfigürasyon

Eğer tanımlı değilse <kbd>app/middlewares.php</kbd> dosyası içerisine Https katmanını tanımlayın.

```php
$c['middleware']->register(
    [
        'Https' => 'Http\Middlewares\Https',
    ]
);
```

Katmanın çalışabilmesi için katmanlar içerisine eklenmesi gerekir.

```php
$c['middleware']->add(
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

Dosya içerisindeki <b>maintenance</b> anahtarları domain adresinin bakıma alınıp alınmadığını kontrol eder, <b>regex</b> anahtarı ise geçerli domain adresleriyle eşleşme yapılabilmesine olanak sağlar. Domain adresinize uygun düzenli ifadeyi regex kısmına girin. Domain adresinizi route yapısına tutturmak <kbd>app/routes.php</kbd> dosyası içerisinde domain grubunuza ait <b>domain</b> ve <b>middleware</b> anahtarlarını aşağıdaki gibi güncelleyin.

```php
$c['router']->group(
    [
        'name' => 'GenericUsers',
        'domain' => 'sub.domain.com', 
        'middleware' => array('Maintenance')
    ],
    function () {

        $this->attach('(.*)');
    }
);
```

Şimdi test için bakım konfigürasyon dosyanızdaki ilgili domain adına adına ait <b>maintenance</b> değerini <b>down</b> olarak güncelleyin,

```php
php task app down subdomain
```

ve ardından alan adınızın bakıma alınıp alınmadığını sayfayı ziyaret ederek kontrol edin.

```php
http://sub.domain.com/
```

Herşey yolunda ise <kbd>resources/templates/maintenance.php</kbd> dosyası içerisinde bulunan <b>Service Unavailable</b> yazısı ile karşılaşmanız gerekir.