
## View Katmanı

Layer paketini kullanarak kontrolör dosyaları ile yönetilebilen (HMVC) view şablonları oluşturmayı sağlar.

#### Kurulum

```php
http://github.com/obullo/http-middlewares/
```

Eğer katman mevcut değilse yuklarıdaki kaynaktan <kbd>View.php</kbd> dosyasını uygulamanızın <kbd>app/classes/Http/Middlewares/</kbd> klasörüne kopyalayın.

#### Konfigürasyon

Eğer tanımlı değilse <kbd>app/middlewares.php</kbd> dosyası içerisine katmanı aşağıdaki gibi tanımlayın.

```php
$middleware->register(
    [
        'View' => 'Http\Middlewares\View',
    ]
);
```

Katmanın çalışabilmesi için katmanlar içerisine eklenmesi gerekir.

```php
$middleware->add(
    [
        // 'Router',
        'View',
    ]
);
```

#### Çalıştırma

<kbd>app/classes/View</kbd> klasörü altında ihtiyaçlarınıza göre aşağıdaki gibi bir şema oluşturun.

```php

namespace View;

Trait Base
{
    public function __invoke()
    {
        $this->view->withData(
            [
                'header' => $this->layer->get('views/header'),
                'footer' => $this->layer->get('views/footer')
            ]
        );
    }
}
```

ve oluşturduğunuz şemayı kontrolör dosyalarından çağırın.

```php

use Obullo\Http\Controller;
use View\Base;

class Welcome extends Controller
{
    use Base;

    public function index()
    {
        $this->view->load('welcome');
    }
}
```