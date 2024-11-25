# 13. Param Converter & 404's

## 学習日

- 2024/11/25

## 学習リソース

- Symfonycasts: Doctrine, Symfony 6 & the Database
- [#13 Param Converter & 404's](https://symfonycasts.com/screencast/symfony-doctrine/param-converter)

## 404 エラー

- 現在は、存在しないページ (例: `/mix/999`) にアクセスすると 500 エラーが発生する。
- これを 404 エラーに変更するために、`VinylMix` エンティティを取得する際に、`createNotFoundException()` を使う。

```php
class MixController extends AbstractController
{
    public function show($id, VinylMixRepository $mixRepository): Response
    {
        if (!$mix) {
            throw $this->createNotFoundException('Mix not found');
        }
    }
}
```

## ParamConverter

- 上記の処理は、`ParamConverter` を使ってさらに簡単に書くことができる。
- `MixController` クラスの `show()` メソッドを以下のように修正する。

```php
    public function show(VinylMix $mix): Response
    {
        return $this->render('mix/show.html.twig', [
            'mix' => $mix,
        ]);
    }
```

- Symfony 6.2 以降では、`ParamConverter` がデフォルトでインストールされている。
- それ以前のバージョンでは以下のコマンドで ` sensio/framework-extra-bundle` をインストールする。

```bash
composer require sensio/framework-extra-bundle
```

- このライブラリを使うことで、`VinylMix` エンティティを取得するためのコードを書かなくても、自動的にエンティティを取得できる。
- そして、存在しないエンティティにアクセスした場合は、自動的に 404 エラーが発生する。
