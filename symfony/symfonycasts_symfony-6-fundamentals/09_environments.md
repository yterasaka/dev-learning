# 09. Environments

## 学習日

- 2024/11/14

## 学習リソース

- Symfonycasts: Symfony 6 Fundamentals: Services, Config & Environments
- [#09 Environments](https://symfonycasts.com/screencast/symfony6-fundamentals/environments)

## App_ENV 変数

- プロジェクトのルートディレクトリにある `.env` ファイルで、`APP_ENV` 変数を設定することで、環境を設定できる。
- `APP_ENV` 変数には、`dev`、`prod`、`test` のいずれかを設定する。

```.env
# In all environments, the following files are loaded if they exist,
# the latter taking precedence over the former:
#
#  * .env                contains default values for the environment variables needed by the app
#  * .env.local          uncommitted file with local overrides
#  * .env.$APP_ENV       committed environment-specific defaults
#  * .env.$APP_ENV.local uncommitted environment-specific overrides
#
# Real environment variables win over .env files.
#
# DO NOT DEFINE PRODUCTION SECRETS IN THIS FILE NOR IN ANY OTHER COMMITTED FILES.
#
# Run "composer dump-env prod" to compile .env files for production use (requires symfony/flex >=1.2).
# https://symfony.com/doc/current/best_practices.html#use-environment-variables-for-infrastructure-configuration
###> symfony/framework-bundle ###
APP_ENV=dev
APP_SECRET=4777a99cd6c61ce84969bd1338737c38
###< symfony/framework-bundle ###
```

- ここで設定された `APP_ENV` 変数は、`public/index.php` ファイルで使用される。

```php
<?php
use App\Kernel;
require_once dirname(__DIR__).'/vendor/autoload_runtime.php';
return function (array $context) {
    return new Kernel($context['APP_ENV'], (bool) $context['APP_DEBUG']);
};
```

- そして、`Kernel` クラスは `src/Kernel.php` ファイルで定義されている。

```php

<?php
namespace App;

use Symfony\Bundle\FrameworkBundle\Kernel\MicroKernelTrait;
use Symfony\Component\HttpKernel\Kernel as BaseKernel;

class Kernel extends BaseKernel
{
    use MicroKernelTrait;
}
```

## config/packages/{ENV} ディレクトリ

- `Kernel` の仕事は、アプリにあるすべてのサービスとルートをロードすることである。
- このメソッドは `configureContainer()` メソッドで定義されている。

```php
private function configureContainer(ContainerConfigurator $container, LoaderInterface $loader, ContainerBuilder $builder): void
    {
        $configDir = $this->getConfigDir();

        $container->import($configDir.'/{packages}/*.{php,yaml}');
        $container->import($configDir.'/{packages}/'.$this->environment.'/*.{php,yaml}'); // ここで環境に応じたファイルを読み込む

        if (is_file($configDir.'/services.yaml')) {
            $container->import($configDir.'/services.yaml');
            $container->import($configDir.'/{services}_'.$this->environment.'.yaml');
        } else {
            $container->import($configDir.'/{services}.php');
        }
    }
```

## when@{ENV} Config

- `config/packages` ディレクトリにあるファイルでは、`when@{ENV}` という構文を使用して、環境に応じた設定を行うことができる。

```yaml
monolog:
  channels:
    - deprecation # Deprecations are logged in the dedicated "deprecation" channel when it exists

when@dev:
  monolog:
    handlers:
      main:
        type: stream
        path: "%kernel.logs_dir%/%kernel.environment%.log"
        level: debug
        channels: ["!event"]
      # uncomment to get logging in your browser
      # you may have to allow bigger header sizes in your Web server configuration
      #firephp:
      #    type: firephp
      #    level: info
      #chromephp:
      #    type: chromephp
      #    level: info
      console:
        type: console
        process_psr_3_messages: false
        channels: ["!event", "!doctrine", "!console"]

when@test:
  monolog:
    handlers:
      main:
        type: fingers_crossed
        action_level: error
        handler: nested
        excluded_http_codes: [404, 405]
        channels: ["!event"]
      nested:
        type: stream
        path: "%kernel.logs_dir%/%kernel.environment%.log"
        level: debug

when@prod:
  monolog:
    handlers:
      main:
        type: fingers_crossed
        action_level: error
        handler: nested
        excluded_http_codes: [404, 405]
        buffer_size: 50 # How many messages should be saved? Prevent memory leaks
      nested:
        type: stream
        path: php://stderr
        level: debug
        formatter: monolog.formatter.json
      console:
        type: console
        process_psr_3_messages: false
        channels: ["!event", "!doctrine"]
      deprecation:
        type: stream
        channels: [deprecation]
        path: php://stderr
```

## 環境に応じたルーティング

- 環境に応じたルーテインングも同様に設定できる。
- このメソッドは `src/Kernel.php` ファイルの `configureRoutes()` メソッドで定義されている。

```php
private function configureRoutes(RoutingConfigurator $routes): void
    {
        $configDir = $this->getConfigDir();

        $routes->import($configDir.'/{routes}/'.$this->environment.'/*.{php,yaml}');
        $routes->import($configDir.'/{routes}/*.{php,yaml}');

        if (is_file($configDir.'/routes.yaml')) {
            $routes->import($configDir.'/routes.yaml');
        } else {
            $routes->import($configDir.'/{routes}.php');
        }

        if (false !== ($fileName = (new \ReflectionObject($this))->getFileName())) {
            $routes->import($fileName, 'annotation');
        }
    }
```

- 環境に応じたルーティングの設定は、`config/bundles.php` ファイルで行う。

```php
<?php

return [
    Symfony\Bundle\FrameworkBundle\FrameworkBundle::class => ['all' => true],
    Symfony\Bundle\TwigBundle\TwigBundle::class => ['all' => true],
    Twig\Extra\TwigExtraBundle\TwigExtraBundle::class => ['all' => true],
    Symfony\Bundle\WebProfilerBundle\WebProfilerBundle::class => ['dev' => true, 'test' => true],
    Symfony\Bundle\MonologBundle\MonologBundle::class => ['all' => true],
    Symfony\Bundle\DebugBundle\DebugBundle::class => ['dev' => true],
    Symfony\WebpackEncoreBundle\WebpackEncoreBundle::class => ['all' => true],
    Symfony\UX\Turbo\TurboBundle::class => ['all' => true],
    Knp\Bundle\TimeBundle\KnpTimeBundle::class => ['all' => true],
];
```

- `all` キーはすべての環境で有効になる。
- 個別の環境で有効にする場合は、`dev`、`test`、`prod` などのキーを使用する。
