# 10. Custom Entity Methods & Twig Magic

## 学習日

- 2024/11/25

## 学習リソース

- Symfonycasts: Doctrine, Symfony 6 & the Database
- [#10 Custom Entity Methods & Twig Magic](https://symfonycasts.com/screencast/symfony-doctrine/twig)

## ビルトインのリポジトリメソッド

- リポジトリにはビルトインのメソッドが用意されている。
- 以下の例では、プロパティの並び順を変更する。

```php
class VinylController extends AbstractController
{
    public function browse(VinylMixRepository $mixRepository, string $slug = null): Response
    {
        $mixes = $mixRepository->findBy([], ['votes' => 'DESC']);
    }
}
```

- `findBy()` メソッドの第一引数には検索条件を指定する。空の配列を指定すると全てのエンティティを取得する。
- `findBy()` メソッドの第二引数には並び順を指定する。`votes` プロパティを降順に並び替えている。つまり、`votes` プロパティが多い順に並び替えている。
- その他にも、`findOneBy()` や `find()` などのメソッドが用意されている。

## カスタムメソッドの追加

### 投票数をフォーマットするメソッド

- エンティティにカスタムメソッドを追加することができる。
- 以下の例では、'votes' の表示の前に '+' または '-' を追加するカスタムメソッドを追加している。

```php
class VinylMix
{
    // ...

    public function getVotesString(): string
    {
        $prefix = ($this->votes === 0) ? '' : (($this->votes >= 0) ? '+' : '-');
        return sprintf('%s %d', $prefix, abs($this->votes));
    }
}
```

- 'sprintf' メソッドのフォーマット指定子:

  - `%s`: 文字列
  - `%d`: 整数
  - [sprintf](https://www.php.net/manual/ja/function.sprintf.php)

- 以下のようにして、テンプレートの中でカスタムメソッドを呼び出す。

```twig
{% block body %}
            {% for mix in mixes %}
                <div class="mixed-vinyl-container p-3 text-center">
                    {{ mix.votesString }} votes
                </div>
            {% endfor %}
{% endblock %}
```

### 画像 URL を生成するメソッド

- 以下の例では、画像 URL を生成するカスタムメソッドを追加している。

```php
class VinylMix
{
    public function getImageUrl(int $width): string
    {
        return sprintf(
            'https://picsum.photos/id/%d/%d',
            ($this->getId() + 50) % 1000, // number between 0 and 1000, based on the id
            $width
        );
    }
}
```

- Twig テンプレートで画像 URL を生成するカスタムメソッドを呼び出す。
- `imageUrl()` メソッドは、画像の幅を引数に取る。

```twig
{% block body %}
            {% for mix in mixes %}
                    <img src="{{ mix.getImageUrl(300) }}" alt="Mix album cover">
            {% endfor %}
{% endblock %}
```

## `git grep` コマンド

- `git grep` コマンドを使うと、リポジトリ内のファイルを検索できる。

```bash
git grep MixRepository
```
