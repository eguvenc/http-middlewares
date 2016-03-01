
## ParsedBody Katmanı

Uygulamaya belirli türlerde gönderilen Http isteği gövdelerini çözümler. Gönderilen istek gövdeleri genellikle <kbd>json</kbd> veya <kbd>xml</kbd> biçimleridir.

#### Konfigürasyon

Eğer tanımlı değilse <kbd>app/middlewares.php</kbd> dosyası içerisine ParsedBody katmanını tanımlayın.

```php
$middleware->register(
    [
        'ParsedBody' => 'Http\Middlewares\ParsedBody',
    ]
);
```

Katmanın çalışabilmesi için evrensel katmanlar içerisine eklenmesi gerekir.

```php
$middleware->init(
    [
        'ParsedBody',
        'Router',
    ]
);
```

#### Kurulum

```php
http://github.com/obullo/http-middlewares/
```

Yukarıdaki kaynaktan <kbd>ParsedBody.php</kbd> dosyasını uygulamanızın <kbd>app/classes/Http/Middlewares/</kbd> klasörüne kopyalayın.

#### Çalıştırma

Uygulamanıza bir http json verisi gönderildiğinde gönderilen veri çözümlenerek <kbd>$request->withParsedBody($parsedBody)</kbd> metodu ile <kbd>$request</kbd> nesnesi içerisine kaydedilir.

```php
class ParsedBody implements MiddlewareInterface
{
    public function __invoke(Request $request, Response $response, callable $next = null)
    {        
        $parsedBody = $request->getParsedBody();

        if (empty($parsedBody)) {

            $body = (string)new PhpInputStream('php://input');
            $mediaType = $this->getMediaType($request);

            switch ($mediaType) {
            case 'application/json':
                $parsedBody = json_decode($body, true);
                break;
            case 'application/xml':
                $parsedBody = simplexml_load_string($body);
                break;
            }
            $request = $request->withParsedBody($parsedBody);
        }
        $err = null;

        return $next($request, $response, $err);
    }
}
```

Çözümlenen veriler request nesnesi içerisinden <kbd>$this->request->getParsedBody()</kbd> metodu ile elde edilir.

#### Örnek

Bir <kbd>Welcome</kbd> kontrolör dosyası yaratın ve dosyayı aşağıdaki gibi güncelleyin.

```php
use Obullo\Http\Controller;

class Welcome extends Controller
{
    public function index()
    {
    	echo print_r($this->request->getParsedBody(), true);
    }
}
```

Bir <kbd>curl.php</kbd> dosyası yaratın ve <kbd>Welcome</kbd> kontrolör dosyanıza json türünde bir veri yollayın.

```php
$postData = array(
    'id' => array(2),
    'title' => 'A new post',
    'content' => 'Example content'
);
$ch = curl_init('http://framework/welcome');

curl_setopt_array($ch, array(
    CURLOPT_POST => true,
    CURLOPT_RETURNTRANSFER => true,
    CURLOPT_HTTPHEADER => array(
		'Content-Type: application/json'
    ),
    CURLOPT_POSTFIELDS => json_encode($postData)
));

$response = curl_exec($ch);

if($response === false){
    die(curl_error($ch));
}
var_dump($response);

//  /var/www/html/curl.php
```

Curl dosyanızı çalıştırın.

```php
http://localhost/curl.php
```

Herşey yolundaysa beklenen çıktı aşağıdaki gibi olmalıdır.

```php
string(129) "Array ( [id] => Array ( [0] => 2 ) [title] => A new post [content] => Example content ) " 
```
