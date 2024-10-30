# 09. Profiler: Your Debugging Best Friend

## 学習日

- 2024/10/25

## 学習リソース

- Symfonycasts: Harmonious Development with Symfony 6
- [#09 Profiler: Your Debugging Best Friend](https://symfonycasts.com/screencast/symfony6/wildcard-route)

## The debug Pack

- 以下のコマンドを使用して、`debug` パックをインストールする。

```bash
composer require debug
```

## Profiler

### PHP Debugging

- Symfony には、デバッグツールとして `Profiler` が用意されている。
- ブラウザの下部に表示される Web プロファイラーを使用して、リクエストの詳細情報を確認できる。
- パフォーマンスの詳細情報を確認できる。

## dump() / dd()

- `dd()` 関数: 以下のようにコントローラー内で使用して、変数の内容を画面に表示することができる。

```php
class VinylController extends AbstractController
{
    #[Route('/')]
    public function homepage(): Response
    {
        dd($tracks);
    }
}
```

- `dump()` 関数: 以下のようにコントローラー内で使用して、プロファイラーに変数の内容を表示することができる。

```php
class VinylController extends AbstractController
{
    #[Route('/')]
    public function homepage(): Response
    {
        dump($tracks);
    }
}
```

### Twig Debugging

- これらの関数は、Twig テンプレート内でも使用することができる。

```twig
<div>
    Tracks:

    {{ dump(tracks) }}
</div>
```

- Twig では `dump()` を引数なしで使用することで、アクセスできる変数をすべて表示することができる。

```twig
<div>
    Tracks:

    {{ dump() }}
</div>
```

---

参照

- [デバッグのやり方](https://qiita.com/aoyama_mad2007/items/ec464e2031dede790031)
