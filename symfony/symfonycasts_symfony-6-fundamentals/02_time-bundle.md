# 02. New Bundle, New Service: KnpTimeBundle

## 学習日

- 2024/11/08

## 学習リソース

- Symfonycasts: Symfony 6 Fundamentals: Services, Config & Environments
- [#02 New Bundle, New Service: KnpTimeBundle](https://symfonycasts.com/screencast/symfony6-fundamentals/time-bundle)

## Twig で日付を表示する

- `createdAt` で日付を表示する。

```twig
<div class="container">
    <div>
        <h2 class="mt-5">Mixes</h2>
        <div class="row">
            {% for mix in mixes %}
            <div class="col col-md-4">
                <div class="mixed-vinyl-container p-3 text-center">
                    <span>{{ mix.genre }}</span>
                    |
                    <span>{{ mix.createdAt|date('Y-m-d') }}</span>
                </div>
            </div>
            {% endfor %}
        </div>
    </div>
</div>
```

## KnpTimeBundle

- "10 minutes ago" のような日付を表示するための Bundle。
- [knplabs/knp-time-bundle](https://github.com/KnpLabs/KnpTimeBundle)

### インストール

```bash
composer require knplabs/knp-time-bundle
```
