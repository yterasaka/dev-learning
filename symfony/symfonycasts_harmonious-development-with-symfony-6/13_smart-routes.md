# 13. Smart Routes: GET-only & Validate {Wildcards}

## 学習日

- 2024/10/30

## 学習リソース

- Symfonycasts: Harmonious Development with Symfony 6
- [#13 Smart Routes: GET-only & Validate {Wildcards}](https://symfonycasts.com/screencast/symfony6/route-requirements#play)

## メソッドの指定

- ルートにメソッドを指定することで、そのメソッドのみが許可されるようになる。
- メソッドを指定しない場合、すべてのメソッドが許可される。

```php
<?php

class SongController extends AbstractController
{
    #[Route('/api/songs/{id}', methods: ['GET'])]
    public function getSong($id): Response
    {
    }
}
```

## メソッドを確認するコマンド

- `debug:router` コマンドを使用して、ルートのメソッドを確認できる。

```bash
php bin/console debug:router
```

- `php bin/console router:match` コマンドを使用して、ルートのメソッドを確認できる。

```bash
php bin/console router:match /api/songs/1
```

- `--method=POST` オプションを使用して、POST メソッドを指定することもできる。

```bash
php bin/console router:match /api/songs/1 --method=POST
```

## 正規表現によるバリデーション

- ルートの要件を指定することで、正規表現によるバリデーションを行うことができる。
- 以下の例では `id` が文字列になる。

```php
<?php
// ... lines 3 - 9
class SongController extends AbstractController
{
    #[Route('/api/songs/{id}', methods: ['GET'])]
    public function getSong(int $id): Response
    {
// ... lines 15 - 22
    }
}
```

- `id` を数値に限定する場合は、以下のように正規表現を使用する。

```php
<?php
// ... lines 3 - 9
class SongController extends AbstractController
{
    #[Route('/api/songs/{id<\d+>}', methods: ['GET'])]
    public function getSong(int $id): Response
    {
// ... lines 15 - 22
    }
}
```
