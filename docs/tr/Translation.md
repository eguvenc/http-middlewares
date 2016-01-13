
## Translation Katmanı

Uygulamaya gelen http isteklerinin tümü için <b>locale</b> anahtarlı çereze varsayılan yerel dili yada url den gönderilen dili kaydeder.

#### Konfigürasyon

Uygulamanın tüm isteklerinde evrensel olarak çalışan bir katmandır ve <kbd>app/middlewares.php</kbd> dosyası içerisinde tanımlanması gerekir. Eğer tanımlı değilse <kbd>app/middlewares.php</kbd> dosyası içerisine katmanı aşağıdaki gibi tanımlayın.

```php
$c['middleware']->register(
    [
        'Translation' => 'Http\Middlewares\Translation',
    ]
);
```

Katmanın çalışabilmesi için katmanlar içerisine eklenmesi gerekir.

```php
$c['middleware']->add(
    [
        // 'Maintenance',
        // 'TrustedIp',
        'Translation',
        'View',
    ]
);
```

Ayrıca translation paketinin konfigürasyon dosyası <kbd>translator.php</kbd> dosyasını konfigüre etmeyi unutmayın.

#### Kurulum

```php
http://github.com/obullo/http-middlewares/
```

Yukarıdaki kaynaktan <kbd>Translation.php</kbd> dosyasını uygulamanızın <kbd>app/classes/Http/Middlewares/</kbd> klasörüne kopyalayın.

#### Çalıştırma

Uygulamanızı <kbd>http://myproject/en/welcome</kbd> gibi ziyaret ettiğinizde yerel dil <b>locale</b> adlı çereze <b>en</b> olarak kaydedilecektir. Artık geçerli yerel dili <kbd>$this->translator->getLocale()</kbd> fonksiyonu ile çağırabilirsiniz.

#### Url Adresi Dil Desteği

Eğer uygulamanızın <kbd>http://example.com/en/welcome/index</kbd> gibi bir dil desteği ile çalışmasını istiyorsanız aşağıdaki route kurallarını <kbd>app/routes.php</kbd> dosyası içerisine tanımlamanız gerekir.

```php
$c['router']->get('(?:en|es|de)/(.*)', '$1');
$c['router']->get('(?:en|es|de)', 'welcome');
```

* İlk kural dil segmentinden sonra kontrolör, metot ve parametre çalıştırmayı sağlar. ( örn. http://example.com/en/examples )
* İkinci kural ise varsayılan açılış sayfası içindir. ( örn. http://example.com/en/ )

> **Not:** Uygulamanızın desteklediği dilleri düzenli ifadelerdeki parentez içlerine girmeniz gerekir. Yukarıda en,es ve de dilleri örnek gösterilmiştir.

Eğer uygulamanızın bütününe değilde sadece belirli url adreslerinize dil desteği eklemek istiyorsanız [RewriteLocale](RewriteLocale.md) katmanını inceleyin.