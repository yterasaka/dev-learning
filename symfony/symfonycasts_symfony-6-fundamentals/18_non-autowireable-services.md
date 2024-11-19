# 18. Non-Autowireable Services

## 学習日

- 2024/11/19

## 学習リソース

- Symfonycasts: Symfony 6 Fundamentals: Services, Config & Environments
- [#18 Non-Autowireable Services](https://symfonycasts.com/screencast/symfony6-fundamentals/named-autowiring)

## 新しい引数を追加する

- 以下のようにして、新しい引数を追加することができる。
- `DebugCommand` クラスを型として指定することで、サービスを利用可能にする。

```php
use Symfony\Bridge\Twig\Command\DebugCommand;
class MixRepository
{
    public function __construct(
        private DebugCommand $twigDebugCommand,
    ) {}
}
```

- `config/services.yaml` で、`DebugCommand` サービスを定義する。
- `@` 記号を使用することで、文字列ではなくサービス ID として解釈される。

```yaml
services:
  App\Service\MixRepository:
    bind:
      $twigDebugCommand: "@twig.command.debug"
```

### #[Autowire()]

- もしくは、`#[Autowire()]` 属性を使用して、サービスを自動的にバインドすることもできる。
- こっちの方法はシンプルで、最新の Symfony で推奨されている。

```php
class MixRepository
{
    public function __construct(
        #[Autowire(service: 'twig.command.debug')]
        private DebugCommand $twigDebugCommand,
    ) {}
}
```

## 追加した引数を使う

- 以下のようにして、追加した引数を使うことができる。

```php
use Symfony\Component\Console\Input\ArrayInput;
use Symfony\Component\Console\Output\BufferedOutput;
class MixRepository
{
    public function findAll(): array
    {
        $output = new BufferedOutput();
        $this->twigDebugCommand->run(new ArrayInput([]), $output);
        dd($output);
    }
}
```
