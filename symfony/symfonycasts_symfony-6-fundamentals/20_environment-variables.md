# 20. Environment Variables

## 学習日

- 2024/11/20

## 学習リソース

- Symfonycasts: Symfony 6 Fundamentals: Services, Config & Environments
- [#20 Environment Variables](https://symfonycasts.com/screencast/symfony6-fundamentals/environment-variables)

## Authorization Header を追加する

- Authorization Header は、以下のようにサービスに直接追加することができる。

```php
class MixRepository
{
    public function findAll(): array
    {
        return $this->cache->get('mixes_data', function(CacheItemInterface $cacheItem) {
            $response = $this->githubContentClient->request('GET', '/SymfonyCasts/vinyl-mixes/main/mixes.json', [
                'headers' => [
                    'Authorization' => 'Token ghp_foo_bar',
                ]
            ]);
        });
    }
}
```

- しかし、以下のように Authorization Header を `framework.yaml` に移動すれば、自動的にリクエストに追加される。

```yaml
framework:
  http_client:
    scoped_clients:
      githubContentClient:
        base_uri: https://raw.githubusercontent.com
        headers:
          Authorization: "Token ghp_FAKE"
```

```php
class MixRepository
{
    public function findAll(): array
    {
        return $this->cache->get('mixes_data', function(CacheItemInterface $cacheItem) {
            $response = $this->githubContentClient->request('GET', '/SymfonyCasts/vinyl-mixes/main/mixes.json');
        });
    }
}
```

## 環境変数

- 機密性の高い情報をコードに直接書くのはセキュリティ上のリスクがあるため、環境変数を使う。

```yaml
framework:
  http_client:
    scoped_clients:
      githubContentClient:
        base_uri: https://raw.githubusercontent.com
        headers:
          Authorization: "Token %env(GITHUB_TOKEN)%"
```

### `.env` ファイルと `.env.local` ファイル

- 環境変数は `.env` ファイルに設定する。

```env
###< symfony/framework-bundle ###

GITHUB_TOKEN=GITHUB_TOKEN_VALUE
```

- しかし、`.env` ファイルも同様に機密性の高い情報をリポジトリにコミットすることになるため、そのような情報は `.env` ファイルに書かずに `.env.local` ファイルを使う。
- `.env.local` は `.gitignore` に追加されているため、リポジトリにはコミットされない。
- `.env.local` ファイルに環境変数を設定することで、`.env` ファイルを上書きすることができる。

```env.local
###< symfony/framework-bundle ###
GITHUB_TOKEN=GITHUB_TOKEN_VALUE
```
