# 14. The Request Object

## 学習日

- 2024/11/26

## 学習リソース

- Symfonycasts: Doctrine, Symfony 6 & the Database
- [#14 The Request Object](https://symfonycasts.com/screencast/symfony-doctrine/request)

## リクエストオプジェクト

- `Request` オブジェクトは、`RequestStack` サービスを使って取得できる。

```php
use Symfony\Component\HttpFoundation\RequestStack;

public function someMethod(RequestStack $requestStack)
{
    $request = $requestStack->getCurrentRequest();
    // $request からデータを取得
    $data = $request->request->get('some_key');
}
```

- しかし、Symfony のコントローラーでは、`Request` オブジェクトを使うことで、HTTP リクエストに関するデータ（POST データ、GET クエリ、ヘッダーなど）を簡単に取得できる。

```php
use Symfony\Component\HttpFoundation\Request;

public function vote(Request $request)
{
    $data = $request->request->get('direction');
}
```

- `Request` オブジェクトは、Symfony の `HttpFoundation` コンポーネントによって提供されるクラスで、HTTP リクエストに関するすべてのデータをカプセル化している。以下のようなデータが含まれている:

  - クエリパラメータ（GET データ）
  - フォームデータ（POST データ）
  - アップロードされたファイル
  - リクエストヘッダー
  - クライアントの IP アドレス
  - リクエストのパスや URL

- `Request` オブジェクトはサービスではない。代わりに、コントローラーの引数として渡すことで取得できる。
- コントローラーでは以下の 4 つの引数を使用できる:
  - ルートワイルドカード (例: `{id}`)

```php
    #[Route('/mix/{id}', name: 'app_mix_show')]
public function show($id)
{
    // $id に URL の {id} 値が格納される
}
```

- サービス (例: `VinylMixRepository`)

```php
public function someAction(EntityManagerInterface $em)
{
    // サービスを利用
}
```

- エンティティ

```php
public function show(VinylMix $mix)
{
    // {id} に基づいてエンティティが自動で取得される
}
```

- `Request` オブジェクト

```php
public function someAction(Request $request)
{
    $postData = $request->request->get('key');
}
```

## POST データの取得

- `Request` オブジェクトからデータを取得するには、以下のプロパティやメソッドを使う。

```php
// POST データの取得
// get('key')で POST データを取得する。
// 存在しない場合は、デフォルト値を返す。
$data = $request->request->get('direction', 'default_value');

// GET パラメータの取得
$query = $request->query->get('sort', 'asc');

// リクエストヘッダー
$header = $request->headers->get('User-Agent');

// アップロードされたファイル
$file = $request->files->get('uploaded_file');
```

## シンプルなフォームの追加

- `MixController` クラスにフォームを処理するためのメソッドを追加する。

```php
class MixController extends AbstractController
{
    // ...
    #[Route('/mix/{id}/vote', name: 'app_mix_vote', methods: ['POST'])]
    public function vote(VinylMix $mix, Request $request): Response
    {
        $direction = $request->request->get('direction', 'up');

        if ($direction === 'up') {
            $mix->setVotes($mix->getVotes() + 1);
        } else {
            $mix->setVotes($mix->getVotes() - 1);
        }
        dd($mix);
    }
    // ...
}
```

- `templates/mix/show.html.twig` テンプレートにフォームを追加する。
- ここで必要なフォームはシンプルなので、Symfony のフォームコンポーネントを使わずに HTML で記述する。

```twig
{% block body %}
    <div class="container">
        <h1 class="d-inline me-3">{{ mix.title }}</h1>
        <div class="row mt-5">
            <div class="col-12 col-md-4">
                {{ include('mix/_recordSvg.html.twig') }}
            </div>
            <div class="col-12 col-md-8 ps-5">
                TODO: print track count, genre and description

                {{ mix.votesString }} votes

                <form action="{{ path('app_mix_vote', {id: mix.id }) }}" method="POST">
                    <button
                        type="submit"
                        class="btn btn-outline-primary"
                        name="direction"
                        value="up"
                    ><span class="fa fa-thumbs-up"></span></button>
                    <button
                        type="submit"
                        class="btn btn-outline-primary"
                        name="direction"
                        value="down"
                    ><span class="fa fa-thumbs-down"></span></button>
                </form>
            </div>
        </div>
    </div>
{% endblock %}
```
