# 12. Dependency Injection

## 学習日

- 2024/11/18

## 学習リソース

- Symfonycasts: Symfony 6 Fundamentals: Services, Config & Environments
- [#12 Dependency Injection](https://symfonycasts.com/screencast/symfony6-fundamentals/dependency-injection)

## Dependency Injection

- 依存性注入（Dependency Injection）とは、オブジェクトなどの間に生じる依存関係を、オブジェクト内のコードに直接記述するのではなく外部から与えることで、で依存性を下げる手法。
- 例えば、以下のようにしてコントローラーで読み込んでいるサービスを渡すことができる。

```php
// Controller クラス

// ... lines 1 - 12
class VinylController extends AbstractController
{
// ... lines 15 - 33
    public function browse(HttpClientInterface $httpClient, CacheInterface $cache, MixRepository $mixRepository, string $slug = null): Response
    {
// ... lines 36 - 37
        $mixes = $mixRepository->findAll($httpClient, $cache); // ここでサービスを渡している
// ... lines 39 - 43
    }
}
```

```php
// Service クラス

// ... lines 1 - 5
use Symfony\Contracts\Cache\CacheInterface;
use Symfony\Contracts\HttpClient\HttpClientInterface;
class MixRepository
{
    public function findAll(HttpClientInterface $httpClient, CacheInterface $cache): array // ここでサービスを受け取っている
    {
// ... lines 13 - 18
    }
}
```

## 依存関係と引数

- しかし、上記の例ではコントローラー側から `HttpClientInterface $httpClient, CacheInterface $cache` を渡す必要はない。
- サービスに渡す引数は、動作に必要なものだけにするべき。

## 依存性注入とコンストラクタ

- 依存性注入は、コンストラクタを使って行うことが多い。
- 例えば、以下のようにしてコンストラクタを使って依存性注入を行うことができる。

```php
class MixRepository
{
    private $httpClient; // ここでプロパティを定義している
    private $cache; // ここでプロパティを定義している

    public function __construct(HttpClientInterface $httpClient, CacheInterface $cache)
    {
        $this->httpClient = $httpClient; // ここで依存性注入を行っている
        $this->cache = $cache; // ここで依存性注入を行っている
    }

    class MixRepository
    {
    // ... lines 11 - 19
        public function findAll(): array
        {
            // this->httpClient と $this->cache を使って処理を行う
            return $this->cache->get('mixes_data', function(CacheItemInterface $cacheItem) {
    // ... line 23
                $response = $this->httpClient->request('GET', 'https://raw.githubusercontent.com/SymfonyCasts/vinyl-mixes/main/mixes.json');
    // ... lines 25 - 26
            });
        }
    }
}
```

### PHP 8

- PHP 8 からは、以下のようにしてコンストラクタを簡潔に書くことができる。

```php
// ... lines 1 - 8
class MixRepository
{
    public function __construct(
        private HttpClientInterface $httpClient, // ここでプロパティを定義している
        private CacheInterface $cache // ここでプロパティを定義している
    ) {}
}
// ... lines 15 - 24
```
