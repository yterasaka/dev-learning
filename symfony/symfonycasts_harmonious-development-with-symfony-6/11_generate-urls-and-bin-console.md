# 11. Generate URLs & bin/console

## 学習日

- 2024/10/28

## 学習リソース

- Symfonycasts: Harmonious Development with Symfony 6
- [#11 Generate Urls & bin/console](https://symfonycasts.com/screencast/symfony6/generate-urls)

## bin/console

- 以下のコマンドはすべて同じ意味を持つ。

```bash
# 1
php bin/console
# 2
./bin/console
# 3
symfony console
```

## bin/console debug:router

- `debug:router` コマンドを使用すると、アプリケーションのすべてのルートを表示できる。

```bash
php bin/console debug:router
```

## Route に名前を付ける

- `Route` の `Name` は、明示的に指定していない場合、Symfony が自動的に生成する。
- メソッド名が変更されたときを考えて、名前は明示的に指定することが推奨される。

```php
<?php

class VinylController extends AbstractController
{
    #[Route('/', name: 'app_homepage')]
    public function homepage(): Response
    {
    }
}
```

### `Route` 属性

- PhpStorm では、`Route` にカーソルを合わせると、そのルートの情報を表示できる。
- command キーを押したままにすると、そのルートにジャンプできる。

## Twig から URL を生成する

- `php bin/console debug:router` で確認したルート名を使用して、Twig テンプレートで URL を生成できる。

```twig
<!DOCTYPE html>
<html>
    <body>
                <a class="navbar-brand" href="{{ path('app_homepage') }}">
                    <i class="fas fa-record-vinyl"></i>
                    Mixed Vinyl
                </a>
    </body>
</html>
```

- ルートに `slug` がある場合は、`path()` 関数の第 2 引数にパラメータ (値の連想配列) を渡す。

```php
<?php

class VinylController extends AbstractController
{
    #[Route('/browse/{slug}', name: 'app_browse')]
    public function browse(string $slug = null): Response
    {
    }
}
```

```twig
{% block body %}
    <ul class="genre-list ps-0 mt-2 mb-3">
        <li class="d-inline">
            <a class="btn btn-primary btn-sm" href="{{ path('app_browse', {
                slug: 'pop'
            }) }}">Pop</a>
        </li>
        <li class="d-inline">
            <a class="btn btn-primary btn-sm" href="{{ path('app_browse', {
                slug: 'rock'
            }) }}">Rock</a>
        </li>
        <li class="d-inline">
            <a class="btn btn-primary btn-sm" href="{{ path('app_browse', {
                slug: 'heavy-metal'
            }) }}">Heavy Metal</a>
        </li>
    </ul>
{% endblock %}
```

---

参照

- [Symfony の構造　 Generate URLs](https://qiita.com/keitean/items/19140fa4322ab478da67)
