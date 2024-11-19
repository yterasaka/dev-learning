# 17. Named Autowiring & Scoped HTTP Clients

## 学習日

- 2024/11/19

## 学習リソース

- Symfonycasts: Symfony 6 Fundamentals: Services, Config & Environments
- [#17 Named Autowiring & Scoped HTTP Clients](https://symfonycasts.com/screencast/symfony6-fundamentals/named-autowiring)

## scoped_clients

- HTTP クライアントを使って、複数のリクエストを送信する場合、`scoped_clients` を使って、リクエストを分離し、ホスト名を指定することができる。

```yaml
framework:
  http_client:
    scoped_clients:
      githubContentClient:
        base_uri: https://raw.githubusercontent.com
```

- 以下のコマンドで、`scoped_clients` で設定したサービスを確認することができる。

```bash
php bin/console debug:autowiring client
```

- 以下のようにして、`scoped_clients` で設定したサービスを使うことができる。

```php
class MixRepository
{
    public function __construct(
        private HttpClientInterface $githubContentClient,
    ) {}
    public function findAll(): array
    {
        return $this->cache->get('mixes_data', function(CacheItemInterface $cacheItem) {
            $response = $this->githubContentClient->request('GET', '/SymfonyCasts/vinyl-mixes/main/mixes.json');
        });
    }
}
```
