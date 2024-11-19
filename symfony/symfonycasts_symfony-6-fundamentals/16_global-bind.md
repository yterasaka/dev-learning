# 16. Bind Arguments Globally

## 学習日

- 2024/11/19

## 学習リソース

- Symfonycasts: Symfony 6 Fundamentals: Services, Config & Environments
- [#16 Bind Arguments Globally](https://symfonycasts.com/screencast/symfony6-fundamentals/global-bind)

## bind

- サービスでない値 (bool 値など) を引数として渡す場合、`_defaults` セクションに `bind` セクションを追加することで、引数をバインドすることができる。
- それによって、設定を一度書くだけで済むため、繰り返しを避けることができる。
- 型ヒントを指定することで、引数の型を指定することもできる。

```yaml
// ... lines 1 - 12
services:
    # default configuration for services in *this* file
    _defaults:
        autowire: true      # Automatically injects dependencies in your services.
        autoconfigure: true # Automatically registers your services as commands, event subscribers, etc.
        bind:
            'bool $isDebug': '%kernel.debug%'
// ... lines 20 - 32
```

## PHP 8 #[Autowire] 属性

- PHP 8 からは、`services.yaml` に記述する代わりに、`#[Autowire]` 属性を使って、引数をバインドすることもできる。

```php
// ... lines 1 - 6
use Symfony\Component\DependencyInjection\Attribute\Autowire;
// ... lines 8 - 9
class MixRepository
{
    public function __construct(
// ... lines 13 - 14
        #[Autowire('%kernel.debug%')]
        private bool $isDebug
    ) {}
// ... lines 18 - 27
}
```
