# 25. Command: Autowiring & Interactive Questions

## 学習日

- 2024/11/20

## 学習リソース

- Symfonycasts: Symfony 6 Fundamentals: Services, Config & Environments
- [#23 MakerBundle & Autoconfiguration](https://symfonycasts.com/screencast/symfony6-fundamentals/maker-command)

## サービスをコマンド内で利用する

- コマンド内でサービスを利用する場合は Dependency Injection を使う。
- 以下のように、`__construct()` メソッドでサービスを受け取り、プロパティとして保持する。
- 注意: コマンドクラスでは `parent::__construct()` を呼び出す必要がある。

```php
use App\Service\MixRepository;
class TalkToMeCommand extends Command
{
    public function __construct(
        private MixRepository $mixRepository
    )
    {
        parent::__construct(); // 親クラスのコンストラクタ呼び出し
    }
}
```

## インタラクティブな質問

- `$io` オブジェクトを使用して、ユーザーと対話することができる。
- `confirm()` メソッドを使用すると、ユーザーに確認を求めることができる。
- `confirm()` メソッドは、ユーザーが `y` または `yes` を入力すると `true` を返す。
- `$io` オブジェクトでは、複数選択の質問や回答の自動補完など、便利な機能がある。プログレスバーも作成できる。

```php
class TalkToMeCommand extends Command
{
    protected function execute(InputInterface $input, OutputInterface $output): int
    {
        $io->success($message);

        if ($io->confirm('Do you want a mix recommendation?')) {
        }
    }
}
```

## データの取得と表示

- 以下の例では、サービス (MixRepository) を使ってデータを取得し、ランダムなデータを選択する。
- `$io->note()` メソッドを使って、ユーザーに `[NOTE]` としてデータを表示する。

```php
class TalkToMeCommand extends Command
{
    protected function execute(InputInterface $input, OutputInterface $output): int
    {
        $io->success($message);

        if ($io->confirm('Do you want a mix recommendation?')) {
            $mixes = $this->mixRepository->findAll();
            $mix = $mixes[array_rand($mixes)];
            $io->note('I recommend the mix: ' . $mix['title']);
        }
    }
}
```

## 終了コマンドの設定

- コマンドの最後に、`return Command::SUCCESS;` を返すことで、コマンドの終了を示す。
- 失敗時は `return Command::FAILURE;` を返す。

```php

```
