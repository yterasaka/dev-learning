# 08. debug:container & How Autowiring Works

## 学習日

- 2024/11/14

## 学習リソース

- Symfonycasts: Symfony 6 Fundamentals: Services, Config & Environments
- [#08 debug:container & How Autowiring Works](https://symfonycasts.com/screencast/symfony6-fundamentals/cache-config)

## Service Container

- Symfony は、Service Container と呼ばれるものを使用して、サービスを管理する。
- Service Container は、サービスをインスタンス化し、依存関係を解決する。
- Service Container は、`bin/console debug:container` コマンドでデバッグすることができる。

```bash
php bin/console debug:container
```

- コンテナはサービスをインスタンス化する方法に関する命令の大きな配列として機能する。
- 複数の箇所で同じサービスを使用する場合、コンテナは同じインスタンスを返す。

## Autowiring

- `debug:container` ではすべてのサービスが表示されるが、`debug:autowiring` では自動ワイヤリングされたサービスだけが表示される。
- それは、すべてのサービスが自動ワイヤリングされているわけではないからである。

## Servie Aliases

- サービスエイリアスは、サービスのシンボリックリンクのようなものである。
