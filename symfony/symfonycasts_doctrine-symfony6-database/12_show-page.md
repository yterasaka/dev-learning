# 12. Querying for a Single Entity for a "Show" Page

## 学習日

- 2024/11/25

## 学習リソース

- Symfonycasts: Doctrine, Symfony 6 & the Database
- [#12 Querying for a Single Entity for a "Show" Page](https://symfonycasts.com/screencast/symfony-doctrine/show-page)

## ミックスの詳細ページを表示する

### 新しいルートとコントローラの作成

- ミックスの詳細ページを表示するために、`Controller/MixController.php` に `show()` メソッドを追加して新しいルートとコントローラを作成する。

```php
use App\Repository\VinylMixRepository;
// ...
class MixController extends AbstractController
{
    #[Route('/mix/{id}')]
    public function show($id, VinylMixRepository $mixRepository): Response
    {
        $mix = $mixRepository->find($id);
        return $this->render('mix/show.html.twig', [
            'mix' => $mix,
        ]);
    }
}
```

### テンプレートの作成

- 詳細ページを表示するためのテンプレート `mix/show.html.twig` を作成する。

```twig
{% block body %}
    <div class="container">
        <h1 class="d-inline me-3">{{ mix.title }}</h1>
        <div class="row mt-5">
            <div class="col-12 col-md-4">

                <svg width="100%" height="100%" viewBox="0 0 496 496" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
                    // レコード画像のコード
                </svg>
            </div>
            <div class="col-12 col-md-8 ps-5">
                TODO: print track count, genre and description
            </div>
        </div>
    </div>
{% endblock %}
```

## テンプレート部分で重複を避ける

- テンプレート部分で重複を避けるために、重複している部分を個別のテンプレート `mix/_recordSvg.html.twig` として分離する。
- プレフィックスとして付けた `_` は、このファイルが部分のテンプレートであることを慣例的に示している。

```twig
<svg width="100%" height="100%" viewBox="0 0 496 496" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
    <defs>
        <linearGradient x1="50%" y1="0%" x2="50%" y2="100%" id="linearGradient-1">
            <stop stop-color="#C380F3" offset="0%"></stop>
            <stop stop-color="#4A90E2" offset="100%"></stop>
        </linearGradient>
    </defs>
    <g id="Mixed-Vinyl" stroke="none" stroke-width="1" fill="none" fill-rule="evenodd">
        <g id="Group">
            <g id="record-vinyl" fill="#000000" fill-rule="nonzero">
                <path d="M248,144 C190.562386,144 144,190.562386 144,248 C144,305.437614 190.562386,352 248,352 C305.437614,352 352,305.437614 352,248 C352,190.562386 305.437614,144 248,144 L248,144 Z M248,272 C234.745166,272 224,261.254834 224,248 C224,234.745166 234.745166,224 248,224 C261.254834,224 272,234.745166 272,248 C272,261.254834 261.254834,272 248,272 Z M248,0 C111,0 0,111 0,248 C0,385 111,496 248,496 C385,496 496,385 496,248 C496,111 385,0 248,0 Z M248,376 C177.307552,376 120,318.692448 120,248 C120,177.307552 177.307552,120 248,120 C318.692448,120 376,177.307552 376,248 C376,281.947711 362.514324,314.505012 338.509668,338.509668 C314.505012,362.514324 281.947711,376 248,376 Z" id="Shape"></path>
            </g>
            <g id="record-vinyl" transform="translate(144.000000, 144.000000)" fill="url(#linearGradient-1)" fill-rule="nonzero">
                <path d="M104,0 C46.562386,0 0,46.562386 0,104 C0,161.437614 46.562386,208 104,208 C161.437614,208 208,161.437614 208,104 C208,46.562386 161.437614,0 104,0 L104,0 Z M104,128 C90.745166,128 80,117.254834 80,104 C80,90.745166 90.745166,80 104,80 C117.254834,80 128,90.745166 128,104 C128,117.254834 117.254834,128 104,128 Z" id="Shape"></path>
            </g>
            <circle id="Oval" stroke="#979797" cx="248" cy="248" r="235"></circle>
            <circle id="Oval" stroke="#979797" cx="248" cy="248" r="215"></circle>
            <circle id="Oval" stroke="#979797" cx="248" cy="248" r="195"></circle>
            <circle id="Oval" stroke="#979797" cx="248" cy="248" r="175"></circle>
            <circle id="Oval" stroke="#979797" cx="248" cy="248" r="155"></circle>
        </g>
    </g>
</svg>
```

- 分離したテンプレートを `mix/show.html.twig` でインクルードする。

```twig
{% block body %}
        <div class="row mt-5">
            <div class="col-12 col-md-8 ps-5">
                <h2 class="mb-4">{{ mix.trackCount }} songs <small>(genre: {{ mix.genre }})</small></h2>
                <p>{{ mix.description }}</p>
            </div>
        </div>
{% endblock %}
```

- `homepage.html.twig` でも同様に重複している部分を分離したテンプレートでインクルードする。

## Show ページへのリンク

- `MixController.php` で `path()` 関数を使用してルートの名前を渡す。

```php
class MixController extends AbstractController
{
    // ...
    #[Route('/mix/{id}', name: 'app_mix_show')]
    public function show($id, VinylMixRepository $mixRepository): Response
    // ...
}
```

- Twig テンプレートで `path()` 関数を使用してリンクを作成する。

```twig
{% block body %}
// ...
            {% for mix in mixes %}
            <div class="col col-md-4">
                <a href="{{ path('app_mix_show', {
                    id: mix.id
                }) }}" class="mixed-vinyl-container p-3 text-center">
                // ...
                </a>
                // ...
            {% endfor %}
// ...
{% endblock %}
```
