# 04. The Http Client Service

## 学習日

- 2024/11/12

## 学習リソース

- Symfonycasts: Symfony 6 Fundamentals: Services, Config & Environments
- [#04 The Http Client Service](https://symfonycasts.com/screencast/symfony6-fundamentals/http-client)

## HTTPClient Component のインストール

- CRUD 操作を行うために、`symfony/http-client` をインストールする。
- `symfony/http-client` はバンドルではなく、コンポーネントである。

```bash
composer require symfony/http-client
```

## HttpClientInterface service の使用

- 以下のように `HttpClientInterface` サービスを使用する。

```php
use Symfony\Contracts\HttpClient\HttpClientInterface;

class VinylController extends AbstractController
{
    public function browse(HttpClientInterface $httpClient, string $slug = null): Response
    {
        $response = $httpClient->request('GET', 'https://raw.githubusercontent.com/SymfonyCasts/vinyl-mixes/main/mixes.json');
        $mixes = $response->toArray();
    }
}
```

---

参照

- [Http Client](https://symfony.com/doc/current/http_client.html)
