
## Translation Katmanı

Uygulamaya gelen http isteklerinin tümü için <kbd>locale</kbd> anahtarlı çereze varsayılan yerel dili yada url den gönderilen dili kaydeder.

#### Konfigürasyon

Uygulamanın tüm isteklerinde evrensel olarak çalışan bir katmandır ve <kbd>app/middlewares.php</kbd> dosyası içerisinde tanımlanması gerekir. Eğer tanımlı değilse <kbd>app/middlewares.php</kbd> dosyası içerisine katmanı aşağıdaki gibi tanımlayın.

```php
$middleware->register(
    [
        'Translation' => 'Http\Middlewares\Translation',
    ]
);
```

Katmanın çalışabilmesi için evrensel katmanlar içerisine eklenmesi gerekir.

```php
$middleware->init(
    [
        // 'Maintenance',
        // 'TrustedIp',
        'Translation',
        'View',
    ]
);
```

Ayrıca translation paketinin konfigürasyon dosyası <kbd>providers/translator.php</kbd> dosyasını konfigüre etmeyi unutmayın.

#### Kurulum

```php
http://github.com/obullo/http-middlewares/
```

Yukarıdaki kaynaktan <kbd>Translation.php</kbd> dosyasını uygulamanızın <kbd>app/classes/Http/Middlewares/</kbd> klasörüne kopyalayın.

#### Çalıştırma

Uygulamanızı <kbd>http://myproject/en/welcome</kbd> gibi ziyaret ettiğinizde yerel dil <kbd>locale</kbd> adlı çereze <kbd>en</kbd> olarak kaydedilecektir. Artık geçerli yerel dili <kbd>$this->translator->getLocale()</kbd> fonksiyonu ile elde edebilirsiniz.

#### Url Adresi Dil Desteği

Eğer uygulamanızın <kbd>http://example.com/en/welcome/index</kbd> gibi bir dil desteği ile çalışmasını istiyorsanız aşağıdaki route kurallarını <kbd>app/routes.php</kbd> dosyası içerisine tanımlamanız gerekir.

```php
$router->get('(?:en|de|es|tr)', 'welcome');     // example.com/en
$router->get('(?:en|de|es|tr)(/)', 'welcome');  // example.com/en/
$router->get('(?:en|de|es|tr)/(.*)', '$1');     // example.com/en/examples/helloWorld
```

Uygulamanızın desteklediği dilleri düzenli ifadelerdeki parentez içlerine girmeniz gerekir. Yukarıda en,es ve de dilleri örnek gösterilmiştir.

Route kurallarının anlamı,

* İlk iki kural varsayılan açılış sayfası içindir. ( örn. http://example.com/en/ )
* Son kural ise dil segmentinden sonra kontrolör, metot ve parametre çalıştırmayı sağlar. ( örn. http://example.com/en/examples )

Eğer uygulamanızın bütününe değilde sadece belirli url adreslerinize dil desteği eklemek istiyorsanız [RewriteLocale](RewriteLocale.md) katmanını inceleyin.
