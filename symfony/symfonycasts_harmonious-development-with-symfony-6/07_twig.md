# 07. Twig

## 学習日

- 2024/10/24

## 学習リソース

- Symfonycasts: Harmonious Development with Symfony 6
- [#07 Twig](https://symfonycasts.com/screencast/symfony6/twig)

## render()

- `render()` メソッドは、Twig テンプレートをレンダリングするために使用される。
- `render()` メソッドは、`Response` オブジェクトを返す。
- `render()` メソッドの第 1 引数は、レンダリングするテンプレートのパス。
- `render()` メソッドの第 2 引数は、テンプレートに渡す変数。

```php
use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;

class VinylController extends AbstractController
{
    #[Route('/')]
    public function homepage(): Response
    {
        // テンプレートのパス: コントローラーの名前/テンプレートの名前
        return $this->render('vinyl/homepage.html.twig', [
            'title' => 'PB & Jams',
        ]);
    }
```

## テンプレートを作成する

- `templates/` ディレクトリに新しい Twig テンプレートを作成する。

## Twig の 3 つのシンタックス

- `{{ }}`: 変数を表示する。
  - 例: `{{ title }}`
- `{% %}`: 制御構造を表示する。
  - 例: `{% if title %} ... {% endif %}`
- `{# #}`: コメントを表示する。
  - 例: `{# This is a comment #}`

```twig
<h1>{{ title }}</h1>

{# TODO: add an image of the record #}

<div>
    Tracks:

    <ul>
        {% for track in tracks %}
            <li>
                {{ track }}
            </li>
        {% endfor %}
    </ul>
</div>
```

## Sub.keys

- 以下のようにして、連想配列のキーを使用して、配列の値にアクセスできる。

```php
<?php

class VinylController extends AbstractController
{
    #[Route('/')]
    public function homepage(): Response
    {
        $tracks = [
            ['song' => 'Gangsta\'s Paradise', 'artist' => 'Coolio'],
            ['song' => 'Waterfalls', 'artist' => 'TLC'],
            ['song' => 'Creep', 'artist' => 'Radiohead'],
            ['song' => 'Kiss from a Rose', 'artist' => 'Seal'],
            ['song' => 'On Bended Knee', 'artist' => 'Boyz II Men'],
            ['song' => 'Fantasy', 'artist' => 'Mariah Carey'],
        ];
    }
}
```

```twig
    <ul>
        {% for track in tracks %}
            <li>
                {{ track.song }} - {{ track.artist }}
            </li>
        {% endfor %}
    </ul>
```

## AbstractController

- `AbstractController` は Symfony のコントローラークラスを作成する際に使用される 基底クラス（ベースクラス）。開発者がこのクラスを拡張することで、Symfony が提供する便利なメソッドを簡単に利用できる。
- 以下の機能を提供するための基礎クラス:
  - Twig テンプレートのレンダリング（render()）
  - リダイレクト処理（redirectToRoute()）
  - セッションやリクエストの取得
  - サービスコンテナへのアクセス
  - URL 生成（generateUrl()）
- [ベストプラクティス](https://symfony.com/doc/current/best_practices.html#controllers)では AbstractController を継承することを推奨している。
