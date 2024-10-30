# 10. Assets, CSS, Images, etc

## 学習日

- 2024/10/25

## 学習リソース

- Symfonycasts: Harmonious Development with Symfony 6
- [#10 Assets, CSS, Images, etc](https://symfonycasts.com/screencast/symfony6/flex-recipes)

## アセット

- アセット (CSS、JavaScript、画像などの静的ファイル) は `public` ディレクトリに配置される。
- アセットに置いた css ファイルには、以下のようにアクセスできる。

```twig
<!DOCTYPE html>
<html>
    <head>
        <link rel="stylesheet" href="/styles/app.css">
    </head>
</html>
```

## asset() 関数

- `asset()` 関数を使用するには、`Asset` コンポーネントをインストールする必要がある。

```bash
composer require symfony/asset
```

- `asset()` 関数を使用すると、アセットへのパスを生成できる。
- `asset()` 関数は、`public` ディレクトリからの相対パスを返す。

```twig
<!DOCTYPE html>
<html>
    <head>
        <link rel="stylesheet" href="{{ asset('styles/app.css') }}">
    </head>
</html>
```

- `asset()` 関数には 2 つの重要な機能がある。
  - サブディレクトリを自動的に検出し、そのプレフィックスを追加する。
  - 後で CDN に簡単に切り替えられるようにすることができる。
