# 03. Routes, Controllers & Responses

## 学習日

- 2024/10/22

## 学習リソース

- Symfonycasts: Harmonious Development with Symfony 6
- [#03 Routes, Controllers & Responses](https://symfonycasts.com/screencast/symfony6/route-controller)

## Controller

### 新しい PHP クラスを作成する

- `src/Controller/` ディレクトリに新しい PHP クラスを作成する。
- PhpStorm: `Controller/` ディレクトリで右クリック > New > PHP Class を選択する。
- **クラスの `namespace` は必ずディレクトリ構造に合わせる。**

  - `App\` は `src/` ディレクトリを表す。

    ```php
    <?php
    namespace App\Controller;
    class VinylController
    {
    }
    ```

- **ファイルの名前 `file.php` は、必ずクラス名と一致させる。**
  - 上記のコードの例だと `VinylController.php` となる。

### Route

- Symfony でルートを作成する方法はいくつかあるが、ほとんどの場合、属性が使用される。
- 属性を使用する場合は、必ず、use ファイルの先頭にそれに対応するステートメントが必要。

```php
<?php

namespace App\Controller;
use Symfony\Component\Routing\Annotation\Route;

class VinylController
{
    #[Route('/')] # このルートは URL を定義する。
    public function homepage()
    {
        die('Vinyl: Definitely NOT a fancy-looking frisbee!');
    }
}

```

### Response

- コントローラーの主な責務は、リクエストに対応する HTTP レスポンス を返すこと。

#### HttpFoundation

- [HttpFoundation](https://symfony.com/doc/current/components/http_foundation.html) は、HTTP で扱うデータをオブジェクトとして扱うことができるようになるコンポーネント。
- `Response` の中には、ユーザーに返したいものをなんでも入れることができる。
- クラスまたはインターフェースを参照するたびに、作業 use 中のファイルの先頭にステートメントを追加する必要がある（PhpStorm に自動補完させると、use ステートメントが自動的に追加される）。

```php
<?php

namespace App\Controller;

use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;

class VinylController
{
    #[Route('/')]
    public function homepage()
    {
        return new Response('Title: "PB and Jams"');
    }
}
```

---

参照

- [HTTP のやりとりに使うデータを扱う、 "HttpFoundation"](https://qiita.com/ippey_s/items/f9690ba3311505ce0ede)
