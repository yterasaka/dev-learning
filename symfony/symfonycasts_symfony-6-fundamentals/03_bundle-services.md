# 03. Finding & Using Services from a Bundle

## 学習日

- 2024/11/08

## 学習リソース

- Symfonycasts: Symfony 6 Fundamentals: Services, Config & Environments
- [#03 Finding & Using Services from a Bundle](https://symfonycasts.com/screencast/symfony6-fundamentals/bundle-services)

## サービスの検索

- Bundle が提供するサービスを確認するには、以下のコマンドを実行する。

```bash
php bin/console debug:autowiring
```

- 特定のサービスを検索 ('time') するには、以下のコマンドを実行する。

```bash
php bin/console debug:autowiring time
```

## DateTimeFormatter Service を使用する

- `use Knp\Bundle\TimeBundle\DateTimeFormatter;` を追加する。
- `DateTimeFormatter` をメソッドの引数に追加し、名前をつける。

```php
use Knp\Bundle\TimeBundle\DateTimeFormatter;
class VinylController extends AbstractController
{
    public function browse(DateTimeFormatter $timeFormatter, string $slug = null): Response
    {
        foreach ($mixes as $key => $mix) {
            $mixes[$key]['ago'] = $timeFormatter->formatDiff($mix['createdAt']);
        }
        dd($mixes);
    }
}
```

- Twig テンプレートで `ago` を表示する。

```twig
<div class="container">
    <div>
        <h2 class="mt-5">Mixes</h2>
        <div class="row">
            {% for mix in mixes %}
            <div class="col col-md-4">
                <div class="mixed-vinyl-container p-3 text-center">
                    <span>{{ mix.genre }}</span>
                    |
                    <span>{{ mix.ago }}</span>
                </div>
            </div>
            {% endfor %}
        </div>
    </div>
</div>
```

## Twig Filter を使用する

- Twig で使用できる Filter を確認する。

```bash
php bin/console debug:twig
```

- 上記のように `DateTimeFormatter` を使用する代わりに、Twig Filter 'ago' を使用することで、同じ機能が簡単に実装できる。

```twig
class VinylController extends AbstractController
{
    #[Route('/browse/{slug}', name: 'app_browse')]
    public function browse(string $slug = null): Response
    {
        $genre = $slug ? u(str_replace('-', ' ', $slug))->title(true) : null;
        $mixes = $this->getMixes();

        return $this->render('vinyl/browse.html.twig', [
            'genre' => $genre,
            'mixes' => $mixes,
        ]);
    }
}
```
