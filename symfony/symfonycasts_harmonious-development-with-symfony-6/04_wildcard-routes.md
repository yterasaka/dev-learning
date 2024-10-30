# 04. Wildcard Routes

## 学習日

- 2024/10/22

## 学習リソース

- Symfonycasts: Harmonious Development with Symfony 6
- [#04 Wildcard Routes](https://symfonycasts.com/screencast/symfony6/wildcard-route)

## Wildcard Routes

- 以下のように、Wildcard Routes を使用することで、動的な URL を定義できる。

```php
// ... lines 1 - 7
class VinylController
{
// ... lines 10 - 15
    #[Route('/browse/{slug}')]
    public function browse(): Response
    {
        return new Response('Breakup vinyl? Angsty 90s rock? Browse the collection!');
    }
}
```

- {slug} は任意の文字列を受け入れることができるが、一般的に slug は URL の一部として使用される文字列を指す。
- {slug} は $slug として引数を受け取ることができる。

```php
// ... lines 1 - 7
class VinylController
{
// ... lines 10 - 15
    #[Route('/browse/{slug}')]
    public function browse($slug): Response
    {
        return new Response('Genre: '.$slug);
        //return new Response('Breakup vinyl? Angsty 90s rock? Browse the collection!');
    }
// ... lines 23 - 24
```

- 引数にはオプションで型宣言を行うことができる。

```php
// ... lines 1 - 7
class VinylController
{
// ... lines 10 - 15
    #[Route('/browse/{slug}')]
    public function browse(string $slug): Response
    {
// ... lines 19 - 21
    }
// ... lines 23 - 24
```

### Symfony's String Component

#### `str_replace()` Function

- str_replace( $検索文字列 , $置換後文字列 , $検索対象文字列 )
- 検索した文字列に一致した全ての文字列を置換。

```php
// ... lines 1 - 7
class VinylController
{
// ... lines 10 - 15
    #[Route('/browse/{slug}')]
    public function browse(string $slug): Response
    {
        $title = str_replace('-', ' ', $slug); // 'rock-music' => 'rock music'
        return new Response('Genre: '.$title);
// ... line 23
    }
// ... lines 25 - 26
```

#### `u()` Function

- `u()`関数は、Symfony が提供している文字列操作のための関数。
- 使い方は [Creating and Manipulating Strings](https://symfony.com/doc/current/string.html#methods-to-change-case) を参照。
- 以下の例の `title` メソッドは、文字列の各単語の先頭を大文字に変換する（タイトルケース）。（[Methods to Change Case](https://symfony.com/doc/current/string.html#methods-to-change-case)）

```php
class VinylController
{

    #[Route('/browse/{slug}')]
    public function browse(string $slug = null): Response
    {
        if ($slug) {
            $title = 'Genre: '.u(str_replace('-', ' ', $slug))->title(true);
        } else {
            $title = 'All Genres';
        }

        return new Response($title);
    }
```

---

参照

- [Creating and Manipulating Strings](https://symfony.com/doc/current/string.html)
