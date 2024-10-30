# 05. Symfony Flex: Aliases, Packs & Recipes

## 学習日

- 2024/10/24

## 学習リソース

- Symfonycasts: Harmonious Development with Symfony 6
- [#05 Symfony Flex: Aliases, Packes & Recipes](https://symfonycasts.com/screencast/symfony6/flex)

## テンプレートライブラリ (Twig) のインストール

以下のコマンドで、テンプレートライブラリ (Twig) をインストールする。

```bash
composer require templates
```

## symfony/flex

- Symfony Flex は Composer プラグイン。
- 3 つの主要な機能を提供する。

## Flex Aliases

- Composer パッケージのエイリアスを提供する。
- 例: `symfony/twig-pack` は `templates` としてエイリアスされる。

## Flex Packs

- Composer パッケージのグループ化。
- 例: `symfony/twig-pack` は `twig` 、 `twig-bundle` 、`extra-bundle` をインストールする。

```json
{
  "name": "symfony/twig-pack",
  "type": "symfony-pack",
  "license": "MIT",
  "description": "A Twig pack for Symfony projects",
  "require": {
    "symfony/twig-bundle": "*",
    "twig/twig": "^2.12|^3.0",
    "twig/extra-bundle": "^2.12|^3.0"
  }
}
```

## Flex Recipes

- Symfony Recipes は Symfony Flex が利用する自動化のレシピ。
- Symfony Flex がインストールされている Composer でライブラリをインストールした際、Symfony Recipes のリポジトリ 内に対応するレシピが存在していれば、その内容に従って色々なタスクを自動で実行してくれる。

---

参照

- [Symfony Flex/Recipes の仕組みをおさらいしてみよう](https://zenn.dev/ttskch/articles/13013224b61531)
