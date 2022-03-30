## Code style

### Отступы
1 таб или 4 пробела. Применимость: везде, HTML, CSS, JS, C#

### Кавычки
Одинарные кавычки предпочтительнее. Правда атрибуты HTML тегов и текстовые пропсы JSX в двойных кавычках

```css
@import '~style settings';

.element {
    backround: url('some-img.jpg');
}
```

```js
const myVar = 'text';
```

```html
<div data-dc-avatar='{"name": "Ivan", "endpoint": "/some-api/v1/"}'>
  <img src="avatar.jpg" />
</div>
```

```jsx
import React from 'react';

export const MyElement = (props) => (
  <div>
    <img alt="some alt text" src={props.src} />
  </div>
)
```

### Naming

#### Имена файлов и БЭМ классов

Используй комбинацию **kebab** и **point** case для **файлов**. Используй только **kebab** case для БЭМ **классов**

👎**Плохо**

```
myBlock /
    myBlock.scss
    
my\_block /
    my\_block.scss
```

```scss
$someSuper\_Gap: 10px;

.myBlock {

  &_Element {

  }
}
```


👍**Хорошо**

```
my-block /
    my-block.scss
    my-block.component.js
```

```scss
$some-super-gap: 10px;

.myBlock {

  &_element {

  }
}
```

#### Имена для JS переменных

Используй camel case для объявления переменных, которые завязаны на функциональной или объектной области видимости.

```js
const mySuperVariable = null;
```

Используй snake case, когда речь идёт о константе, которая находится за используемой функциональной или объектной 
области видимости,

```js
const ITEMS = ['ball', 'peach', 'car'];

function coverItems = () => {
  ITEMS.map(item => {
    ...
  })
}
```

### Let или Const

По умолчанию const, при необходимости можно let

