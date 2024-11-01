# 15. The Twig Service & Profiler for API Requests

## 学習日

- 2024/11/01

## 学習リソース

- Symfonycasts: Harmonious Development with Symfony 6
- [#15 Service Objects](https://symfonycasts.com/screencast/symfony6/twig-service)

## The Profiler for API Requests

- ログメッセージは Profiler で確認できる。
- API エンドポイントには、ブラウザで表示される HTML ページがないため、Profiler にアクセスすることができない。
- そのため、API リクエストのログを確認するためには、手動で`/_profiler` にアクセスする必要がある。
- `/_profiler` にアクセスすると、最新の 10 件のリクエストが表示される。
- そのリストから、ログを確認したいリクエストの URL をクリックすると、そのリクエストの詳細を確認できる。

## 手動で Twig テンプレートをレンダリングする

- Twig テンプレートを `render()` メソッドを使用せずにレンダリングする場合は、以下のように `twig` サービスを使用する。

```php
<?php

class VinylController extends AbstractController
{
    #[Route('/', name: 'app_homepage')]
    public function homepage(Environment $twig): Response
    {
        $html = $twig->render('vinyl/homepage.html.twig', [
            'title' => 'PB & Jams',
            'tracks' => $tracks,
        ]);

        return new Response($html);
    }
}
```

- 通常、Twig テンプレートは `render()` メソッドを使用してレンダリングするが、`twig` サービスを使用することもできる。
