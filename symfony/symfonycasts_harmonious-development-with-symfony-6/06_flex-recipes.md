# 06. Flex Recipes

## 学習日

- 2024/10/24

## 学習リソース

- Symfonycasts: Harmonious Development with Symfony 6
- [#06 Flex Recipes](https://symfonycasts.com/screencast/symfony6/flex-recipes)

## Flex Recipes を使用する

- 以下のコマンドを実行すると、インストールされているすべてのレシピを確認できる。

```bash
composer recipes
```

- ターミナルに表示された `installed recipe` の URL にアクセスすると、そのレシピの内容を確認できる。
- レシピには `manifest.json` が含まれており、その内容に従って Composer が自動でタスクを実行する。

## Flex Recipes の更新

- インストールしたレシピに更新がある場合、以下のコマンドで更新できる。

```bash
# レシピの更新を確認
composer recipes

# レシピの更新を実行
composer recipes:update
```
