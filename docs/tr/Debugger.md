
## Çıktı Görüntüleyici

Çıktı görüntüleyici uygulama isteklerinden sonra oluşan ortam bileşenleri ve log verilerini görselleştirir.

#### Nasıl Çalışır ?

Debugger sunucusu çalışırken uygulama ziyaret edilir ve uygulama çalışırken bir başka yeni pencerede <kbd>http://yourproject/debugger</kbd> adresine girilerek bu sayfada http, konsol, ajax log verileri ve ortam bilgileri ( http başlıkları, http gövdesi ) websocket bağlantısı ile dinamik olarak görüntülenir.

#### Konfigürasyon

Çıktı görüntüleyicinin <kbd>app/middlewares.php</kbd> içerisine eklenmesi gerekmez uygulama tarafından katmanlar içerisine kendiliğinden dahil edilir. Fakat çıktı görüntüleyicinin log dosyalarını okuyabilmesi için <kbd>File</kbd> sürücüsünün logger servisinizde aşağıdaki gibi herhangi bir önemlilik seviyesinde tanımlı olması gerekir.

```php
'methods' => [
    ['registerFilter' => ['priority', 'Obullo\Log\Filter\PriorityFilter']],
    ['registerHandler' => [5, 'file']],
]
```

#### Kurulum

```php
http://github.com/obullo/http-middlewares/
```

Yularıdaki kaynaktan <kbd>Debugger.php</kbd> dosyasını uygulamanızın <kbd>app/classes/Http/Middlewares/</kbd> klasörüne kopyalayın.

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

### Windows

#### Çalıştırma

Konsolunuzdan debugger sunucusunu çalıştırın. Bu örnekte Xampp Programı baz alınmıştır.

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
