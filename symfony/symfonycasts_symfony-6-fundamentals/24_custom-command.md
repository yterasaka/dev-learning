# 24. Customizing a Command

## 学習日

- 2024/11/20

## 学習リソース

- Symfonycasts: Symfony 6 Fundamentals: Services, Config & Environments
- [#24 Customizing a Command](https://symfonycasts.com/screencast/symfony6-fundamentals/custom-command)

## コマンドのカスタマイズ

### 名前と説明の変更

- 上部の `#[AsCommand]` 属性で設定する。

```php
#[AsCommand(
    name: 'app:talk-to-me',
    description: 'A self-aware command that can do... only one thing.',
)]
class TalkToMeCommand extends Command
{
    // コマンドのロジックが続く
}
```

### 引数とオプションの設定

- `configure()` メソッドで設定する。

```php
class TalkToMeCommand extends Command
{
    protected function configure(): void
    {
        $this
            ->addArgument('name', InputArgument::OPTIONAL, 'Your name')
            ->addOption('yell', null, InputOption::VALUE_NONE, 'Shall I yell?')
        ;
    }
}
```

- 引数の追加
  - `InputArgument::OPTIONAL`: 引数は任意。
  - 引数を必須にしたい場合は、`InputArgument::REQUIRED` を使用
- オプションの追加

  - `InputOption::VALUE_NONE`: 値を必要としないフラグ型オプション。
  - `InputOption::VALUE_REQUIRED`: オプションが値を受け取る場合。

- 追加した設定は、以下のコマンドで確認できる。

```bash
php bin/console app:talk-to-me --help
```

## `execute()` メソッドのロジック

### 引数とオプションを取得

- `$input` を使用して、引数やオプションを読み取る。

```php
class TalkToMeCommand extends Command
{
    protected function execute(InputInterface $input, OutputInterface $output): int
    {
        $io = new SymfonyStyle($input, $output);
        $name = $input->getArgument('name') ?: 'whoever you are';
        $shouldYell = $input->getOption('yell');
    }
}
```

- `$input->getArgument('name')`: 引数 name の値。
- `$input->getOption('yell')`: オプション --yell の有無を判定。

### 出力の生成

- `SymfonyStyle` クラスを使用して、出力を生成する。

```php
$message = sprintf('Hey %s!', $name);
if ($shouldYell) {
    $message = strtoupper($message);
}
$io->success($message);
```

- コマンドの実行例:

```bash
php bin/console app:talk-to-me ryan

# 結果: Hey ryan!

php bin/console app:talk-to-me ryan --yell

# 結果: HEY RYAN!

php bin/console app:talk-to-me --yell

# 結果: HEY WHOEVER YOU ARE!
```
