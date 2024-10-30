# 08. Twig Inheritance

## 学習日

- 2024/10/25

## 学習リソース

- Symfonycasts: Harmonious Development with Symfony 6
- [#08 Twig Inheritance](https://symfonycasts.com/screencast/symfony6/twig-inheritance)

## Tag / Filter

- Twig のタグやフィルターは、[公式ドキュメント](https://twig.symfony.com/doc/3.x/)で確認できる。

```twig
<!-- タグの使用例 -->
{% if title %}
    <h1>{{ title }}</h1>
{% endif %}

<!-- フィルターの使用例 -->
{{ title|upper }}
```

## Twig の継承

- Twig では、テンプレートの継承をサポートしている。
- `extends` タグを使用して、親テンプレートを指定する。
- `block` タグを使用して、親テンプレートのブロックをオーバーライドする。

```twig
{% extends 'base.html.twig' %}

{% block content %}
    <h1>{{ title }}</h1>
{% endblock %}
```

## `parent()` 関数

- `block` タグの既存のコンテンツを表示するには、`parent()` 関数を使用する。

```twig
{% extends 'base.html.twig' %}

{% block content %}
    {{ parent() }}

    <p>More content here!</p>
{% endblock %}
```
