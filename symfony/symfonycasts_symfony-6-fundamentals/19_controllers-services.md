# 19. Controllers are Services Too

## 学習日

- 2024/11/19

## 学習リソース

- Symfonycasts: Symfony 6 Fundamentals: Services, Config & Environments
- [#19 Controllers are Services Too!](https://symfonycasts.com/screencast/symfony6-fundamentals/controllers-services)

## コントローラもサービス

- Symfony のコントローラークラスは、他のサービスと同じように DI（依存性注入）コンテナで管理されている。
- 特別なのは、アクションメソッドに引数を自動注入できること。
- アクションメソッドに渡せる引数は以下の 2 種類:
  - ルート内のワイルドカード (例: `$slug`)
  - Auto-wiring 可能なサービス (例: `MixRepository`)
- その他の値を渡すには、以下のように `services.yaml` でのバインディング設定が必要。

```yaml
services:
  # default configuration for services in *this* file
  _defaults:
    bind:
      "bool $isDebug": "%kernel.debug%"
```

```php
class VinylController extends AbstractController
{
    public function browse(MixRepository $mixRepository, bool $isDebug, string $slug = null): Response
    {
        dump($isDebug);
    }
}
```

## コンストラクタの追加

- コントローラークラスにコンストラクタを追加することで、サービスを追加もできる。

```php
class VinylController extends AbstractController
{
    public function __construct(
        private bool $isDebug,
        private MixRepository $mixRepository
    )
    {}
    #[Route('/browse/{slug}', name: 'app_browse')]
    public function browse(string $slug = null): Response
    {
        dump($this->isDebug);

        $mixes = $this->mixRepository->findAll();
    }
}
```

- アクションメソッドの引数注入は簡単で便利。ただし、クラス全体で使用する値にはコンストラクタ注入が有効。
