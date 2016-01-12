
## Çıktı Görüntüleyici

Çıktı görüntüleyici uygulama isteklerinden sonra oluşan ortam bileşenleri ve arka plan log verilerini görselleştirir. Debugger modülü aktifken uygulama ziyaret edildir ve uygulama çalışırken bir başka yeni pencerede <kbd>http://yourproject/debugger</kbd> adresine girilerek bu sayfada http, konsol, ajax log verileri ve ortam bilgileri ( http başlıkları, http gövdesi ) websocket bağlantısı ile dinamik olarak görüntülenir.

#### Konfigürasyon

Çıktı görüntüleyicinin log dosyalarını okuyabilmesi için <kbd>File</kbd> sürücüsünün logger servisinizde aşağıdaki gibi herhangi bir önemlilik seviyesinde tanımlı olması gerekir.

```php
'methods' => [
    ['registerFilter' => ['priority', 'Obullo\Log\Filter\PriorityFilter']],
    ['registerHandler' => [5, 'file']],
    .
    .
]
```

#### Middleware

```php
http://github.com/obullo/http-middlewares/
```

Yularıdaki kaynaktan <kbd>Debugger.php</kbd> dosyasını uygulamanızın <kbd>app/classes/Http/Middlewares/</kbd> klasörüne kopyalayın.

### Linux / Mac Kurulumu

#### Kurulum

Aşağıdaki komutu konsoldan çalıştırın.

```php
php task module add debugger
```

#### Kaldırma

```php
php task module remove debugger
```

İşlem bittiğinde debugger modülüne ait dosyalar <kbd>modules/debugger</kbd>  ve <kbd>modules/tasks</kbd> klasörleri altına kopyalanırlar.

#### Çalıştırma

Debugger ın çalışabilmesi için debugger sunucusu arka planda çalıştırmanız gerekir. Bunun için konsolunuza aşağıdaki komutu girin.

```php
php task debugger
```

Debugger sayfası ziyaret edildiğinde javascript kodu istemci olarak websocket sunucusuna bağlanır. Şimdi tarayıcınıza gidip yeni bir pencere açın ve debugger sayfasını ziyaret edin.

```php
http://mylocalproject/debugger
```

Eğer debugger kurulumu doğru gerçekleşti ise aşağıdaki gibi bir sayfa ile karşılaşmanız gerekir.

![Debugger](images/debugger.png?raw=true "Debugger Ekran Görüntüsü")

Websocket bağlantısı bazı tarayıcılarda kendiliğinden kopabilir panel üzerindeki ![Closed](images/socket-closed.png?raw=true "Socket Closed") simgesi debugger sunucusuna ait bağlantının koptuğunu ![Open](images/socket-open.png?raw=true "Socket Open") simgesi ise bağlantının aktif olduğunu gösterir. Eğer bağlantı koparsa verileri sayfa yenilemesi olmadan takip edemezsiniz. Böyle bir durumda debugger sunucunuzu ve tarayıcınızı yeniden başlatmayı deneyin.


### Windows Kurulumu

#### Kurulum

Bu örnekte Xampp Programı baz alınmıştır. Aşağıdaki komutu konsoldan çalıştırın.

```php
C:\xampp\php\php.exe -f "C:\xampp\htdocs\myproject\task" module add debugger
```

#### Kaldırma

```php
C:\xampp\php\php.exe -f "C:\xampp\htdocs\myproject\task" module remove debugger
```

İşlem bittiğinde debugger modülüne ait dosyalar <kbd>modules/debugger</kbd>  ve <kbd>modules/tasks</kbd> klasörleri altına kopyalanırlar.

#### Çalıştırma

Debugger ın çalışabilmesi için debugger sunucusu arka planda çalıştırmanız gerekir. Bunun için konsolunuza aşağıdaki komutu girin.

```php
C:\xampp\php\php.exe -f "C:\xampp\htdocs\myproject\task" debugger
```

Debugger sayfası ziyaret edildiğinde javascript kodu istemci olarak websocket sunucusuna bağlanır. Şimdi tarayıcınıza gidip yeni bir pencere açın ve debugger sayfasını ziyaret edin.

```php
http://mylocalproject/debugger
```

Eğer debugger kurulumu doğru gerçekleşti ise aşağıdaki gibi bir sayfa ile karşılaşmanız gerekir.

![Debugger](images/debugger.png?raw=true "Debugger Ekran Görüntüsü")

Websocket bağlantısı bazı tarayıcılarda kendiliğinden kopabilir panel üzerindeki ![Closed](images/socket-closed.png?raw=true "Socket Closed") simgesi debugger sunucusuna ait bağlantının koptuğunu ![Open](images/socket-open.png?raw=true "Socket Open") simgesi ise bağlantının aktif olduğunu gösterir. Eğer bağlantı koparsa verileri sayfa yenilemesi olmadan takip edemezsiniz. Böyle bir durumda debugger sunucunuzu ve tarayıcınızı yeniden başlatmayı deneyin.