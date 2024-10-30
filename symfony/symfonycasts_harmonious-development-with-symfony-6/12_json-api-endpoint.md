# 12. JSON API Endpoint

## 学習日

- 2024/10/28

## 学習リソース

- Symfonycasts: Harmonious Development with Symfony 6
- [#12 JSON API Endpoint](https://symfonycasts.com/screencast/symfony6/json-api)

## JSON Controller

- `JsonResponse` クラスを使用して、JSON レスポンスを返すことができる。
- `JsonResponse` はデータを自動的に JSON に変換する。

```php
<?php

use Symfony\Component\HttpFoundation\JsonResponse;
class SongController extends AbstractController
{
    #[Route('/api/songs/{id}')]
    public function getSong($id): Response
    {
        // TODO query the database
        $song = [
            'id' => $id,
            'name' => 'Waterfalls',
            'url' => 'https://symfonycasts.s3.amazonaws.com/sample.mp3',
        ];

        return new JsonResponse($song);
        // これは `return $this->json($song);` のショートカット
    }
}
```
