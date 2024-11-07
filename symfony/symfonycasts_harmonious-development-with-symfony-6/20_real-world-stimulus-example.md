# 20. Real-World Stimulus Example

## 学習日

- 2024/11/07

## 学習リソース

- Symfonycasts: Harmonious Development with Symfony 6
- [#20 Real-World Stimulus Example](https://symfonycasts.com/screencast/symfony6/stimulus-example#script)

## stimulus_action()

- コントローラーにアクションを追加して、JavaScript のメソッドを呼び出すことができる。
- stimulus_action() は　`data-action` 属性を追加するためのヘルパー関数。
- 以下のように記述することができる。

```twig
{% extends 'base.html.twig' %}

{% block title %}Create a new Record | {{ parent() }}{% endblock %}

{% block body %}
<div class="container">
            {% for track in tracks %}
            <div class="song-list" {{ stimulus_controller('song-controls') }}>
                <div class="d-flex mb-3">
                    <a href="#" {{ stimulus_action('song-controls', 'play') }}> // メソッドは 'play'
                        <i class="fas fa-play me-3"></i>
                    </a>
                </div>
            </div>
            {% endfor %}
</div>
{% endblock %}
```

## Twig からコントローラーにデータを渡す

- 以下のようにしてデータを渡すことができる。

```php
import axios from 'axios';
export default class extends Controller {
    static values = {
        infoUrl: String
    }
    play(event) {
        console.log(this.infoUrlValue);
        //axios.get()
    }
}
```

- `static values` オプジェクトについては以下を参照。
- [Values](https://stimulus.hotwired.dev/reference/values)

```php
<?php

class SongController extends AbstractController
{
    #[Route('/api/songs/{id<\d+>}', methods: ['GET'], name: 'api_songs_get_one')]
    public function getSong(int $id, LoggerInterface $logger): Response
    {
    }
}
```

```twig
{% block body %}
<div class="container">
            {% for track in tracks %}
            <div class="song-list" {{ stimulus_controller('song-controls', {
                infoUrl: path('api_songs_get_one', { id: loop.index })
            }) }}>
            </div>
            {% endfor %}
</div>
{% endblock %}
```

- `loop.index` を使って、ループのインデックスを取得することができる。
- [The loop variable](https://twig.symfony.com/doc/3.x/tags/for.html#the-loop-variable)
