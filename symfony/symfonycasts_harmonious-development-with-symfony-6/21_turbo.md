# 21. Turbo: Supercharge your App

## 学習日

- 2024/11/08

## 学習リソース

- Symfonycasts: Harmonious Development with Symfony 6
- [#21 Turbo: Supercharge your App](https://symfonycasts.com/screencast/symfony6/turbo)

## symfony/ux-turbo のインストール

```bash
composer require symfony/ux-turbo
```

- `yarn watch` を停止して以下を実行。

```bash
yarn install --force
# or
npm install --force
```

## symfony/ux-turbo とは

- symfony/ux-turbo は hotwired/turbo-drive ([TURBO](https://turbo.hotwired.dev/)) を Symfony アプリケーションに統合するためのブリッジライブラリ。
- つまり、JavaScript を書かずに SPA のスピード感を出せるよ！というコンセプト。
