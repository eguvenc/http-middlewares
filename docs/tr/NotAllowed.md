
## NotAllowed Katmanı

Uygulamaya gelen Http isteklerine göre metot türlerini filtrelemeyi sağlar. Eğer belirlenen http metotları ( get, post, put, delete ) dışında bir istek gelirse istek, <kbd>405 Method Not Allowed</kbd> sayfası ile engellenir.

#### Konfigürasyon

Framework çekirdeğinde çalışan bir katmandır. Eğer tanımlı değilse <kbd>app/middlewares.php</kbd> dosyası içerisine NotAllowed katmanını tanımlayın.

```php
$c['middleware']->register(
    [
        'NotAllowed' => 'Http\Middlewares\NotAllowed',
    ]
);
```

Anotasyon özelliğinin kullanabilmesi için anotasyonların <kbd>app/$env/config.php</kbd> dosyasından aşağıdaki gibi açık olması gerekir.

```php
'extra' => [
    'annotations' => true,
],
```

#### Kurulum

```php
http://github.com/obullo/http-middlewares/
```

Yukarıdaki kaynaktan <kbd>NotAllowed.php</kbd> dosyasını uygulamanızın <kbd>app/classes/Http/Middlewares/</kbd> klasörüne kopyalayın.

#### Çalıştırma

NotAllowed katmanı anotasyonlar yardımı ile aşağıdaki gibi controller sınıfı içerisinden

```php
/**
 * Index
 *
 * @middleware->method("get", "post");
 * 
 * @return void
 */
public function index()
{
    // ..
}
```

çalıştırılabilir. Eğer index metoduna get veya post haricinde bir istek türü gelirse <kbd>GET Method Not Allowed</kbd> hatası almanız gerekir. NotAllowed katmanı aşağıdaki gibi bir route kuralı içerisinden de çalıştırılabilir.

```php
$c['router']->group(
    [
    	'middleware' => array()
    ],
    function () {

        $this->match(['post', 'put'], 'welcome');
    }
);
```

Yukarıdaki örnekte <kbd>/welcome</kbd> adresine yalnızca <kbd>POST</kbd> ve <kbd>DELETE</kbd> istekleriyle erişilebilir. 

```php
http://example.com/welcome
```

Uygulamanızı yukarıdaki gibi bir GET isteği ile ziyaret ettiğinizde bir <kbd>GET Method Not Allowed</kbd> hatası almanız gerekir.