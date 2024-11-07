# 19. Stimulus: Sensible, Beautiful JavaScript

## 学習日

- 2024/11/07

## 学習リソース

- Symfonycasts: Harmonious Development with Symfony 6
- [#19 Stimulus: Sensible, Beautiful JavaScript](https://symfonycasts.com/screencast/symfony6/stimulus)

## Stimulus とは

- Stimulus は JavaScript フレームワークで、Web ページに富んだインタラクティブな機能を簡単に追加できる。

## Stimulus の使い方

- twig テンプレートに `data-controller` 属性を追加することで、Stimulus を使うことができる。

```js
import { Controller } from "stimulus";

export default class extends Controller {
  static targets = ["name", "output"];

  greet() {
    this.outputTarget.textContent = `Hello, ${this.nameTarget.value}!`;
  }
}
```

```twig
<div data-controller="hello">
  <input data-hello-target="name" type="text">

  <button data-action="click->hello#greet">
    Greet
  </button>

  <span data-hello-target="output">
  </span>
</div>
```

## stimulus_controller()

- 以下のようなヘルパー関数を使用して記述することもできる。

```twig
<div {{ stimulus_controller('hello') }}></div>
```
