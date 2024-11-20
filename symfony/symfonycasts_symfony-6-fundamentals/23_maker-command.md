# 23. MakerBundle & Autoconfiguration

## 学習日

- 2024/11/20

## 学習リソース

- Symfonycasts: Symfony 6 Fundamentals: Services, Config & Environments
- [#23 MakerBundle & Autoconfiguration](https://symfonycasts.com/screencast/symfony6-fundamentals/maker-command)

## MakerBundle

- MakerBundle は、Symfony でコードを生成するためのツール。
- 主にローカル環境で使用し、本番環境には不要なので `--dev` フラグでインストールする。

```bash
composer require maker --dev
```

- 以下のコマンドで、コマンドの一覧を確認できる。

```bash
php bin/console
```

## カスタムコマンドの作成

- 以下のコマンドで、カスタムコマンドを作成できる。

```bash
php bin/console make:command

# コマンド名を入力する (例: app:talk-to-me)
```

- 作成されたファイルは、`src/Command/TalkToMeCommand.php` に保存される。

## カスタムコマンドの内容

- 作成されたファイルの内容は以下の通り。

```php
<?php

namespace App\Command;

use Symfony\Component\Console\Attribute\AsCommand;
use Symfony\Component\Console\Command\Command;
use Symfony\Component\Console\Input\InputArgument;
use Symfony\Component\Console\Input\InputInterface;
use Symfony\Component\Console\Input\InputOption;
use Symfony\Component\Console\Output\OutputInterface;
use Symfony\Component\Console\Style\SymfonyStyle;

#[AsCommand(
    name: 'app:talk-to-me',
    description: 'Add a short description for your command',
)]
class TalkToMeCommand extends Command
{
    protected function configure(): void
    {
        $this
            ->addArgument('arg1', InputArgument::OPTIONAL, 'Argument description')
            ->addOption('option1', null, InputOption::VALUE_NONE, 'Option description')
        ;
    }

    protected function execute(InputInterface $input, OutputInterface $output): int
    {
        $io = new SymfonyStyle($input, $output);
        $arg1 = $input->getArgument('arg1');

        if ($arg1) {
            $io->note(sprintf('You passed an argument: %s', $arg1));
        }

        if ($input->getOption('option1')) {
            // ...
        }

        $io->success('You have a new command! Now make it your own! Pass --help to see your options.');

        return Command::SUCCESS;
    }
}
```

- `#[AsCommand(...)]` 属性でコマンドの名前と説明を指定。
- `configure()` メソッドで引数とオプションを設定。
- `execute()` メソッドでコマンドの処理を記述。

## カスタムコマンドの実行

- 作成したコマンドは、以下のコマンドで実行できる。

```bash
php bin/console app:talk-to-me
```

## Autoconfiguration (自動設定)

- Autoconfiguration は、Symfony がサービスを自動的に登録する機能。
- `config/services.yaml` で設定されている。

```yaml
services:
  # default configuration for services in *this* file
  _defaults:
    autowire: true # Automatically injects dependencies in your services.
    autoconfigure: true # Automatically registers your services as commands, event subscribers, etc.
```

### Autoconfiguration の役割

- クラスの役割を判別: 例えば、`Command` クラスを拡張していると、Symfony はそれを「コンソールコマンド」として認識。
- 必要なタグを自動付与: 内部的には、サービスに console.command というタグを付与。
- 手動登録の手間を削減: services.yaml にコマンドクラスを明示的に登録する必要はない。
