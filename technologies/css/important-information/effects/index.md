# Эффекты интеррактивных элементов

## Cостояния

Если дизайнер нарисовал кнопку, но забыл или не захотел обозначать на дизайне состояния (disable, active, hover),
то это не значит, что реализация этих состояний не требуется. Конечно, для выхода из данной ситуации вы
можете использовать своё чувство прекрасного, но лишь после того, как убедитесь, что других вариантов не осталось.
Первым делом вы должны обратиться к дизайнеру и попросить его отобразить в дизайне эти состояния.
Помните, любой элемент с которым пользователь может взаимодействовать должен иметь состояния отображения!
А также дополнительные css свойства для лучшего восприятия, например, cursor: pointer.

Хорошая практика — сбрасывать hover стили для тач-устройств.
Иначе при нажатии на какой-то элемент ховер-стили будут залипать — телефон не знает, когда вы отводите палец в сторону.

```scss
@media (min-width: $big-tablet-window-width) {
  &:hover {
    text-decoration: underline;
  }
}
```

## Transitions

В своей работе старайтесь делать анимацию стилей для наведения по принципу «появляется быстро, пропадает медленно».
Это позволяет пользователю быстро увидеть реакцию на свои действия и не дожидаться окончания анимации.

```scss
.link {
  color: #ffffff;
  text-decoration-color: #2e9aff;
  /* Скорость исчезновения фонового цвета */
  transition: background-color 0.5s linear;

  @media (min-width: $big-tablet-window-width) {
    &:hover {
      background-color: #2e9aff;
      /* Скорость изменения фонового цвета на голубой */
      transition: background-color 0.1s linear;
    }
  }
}
```

Также, мы часто используем временные константы для таких эффектов, чтобы сохранять консистентность анимации:

```js
export const DEFAULT_ELEMENT_DELAY = "0.1s";
export const MEDIUM_ELEMENT_DELAY = "0.3s";
export const LONG_ELEMENT_DELAY = "0.5s";
```

```scss
$defaultElementDelay: 0.1s;
$mediumElementDelay: 0.3s;
$longElementDelay: 0.5s;
```

По возможности старайтесь не использовать слово all для описания перехода (transition: all .3s).
Да, это проще на первоначальном этапе, но позже из-за этого в какой-то момент могут начать плавно изменяться свойства,
которые не должны этого делать. Ну и вообще, когда браузер встречает слово all,
он начинает перебирать каждое свойство элемента в поисках необходимого. Это ненужная нагрузка.

👎**Плохо:**

```scss
.element {
  transition: all 0.5s ease;
}
```

👍**Хорошо:**

```scss
.element {
  transition: background-color 0.3s linear, color 0.3s linear;
}
```

👍👍**Отлично:**

```scss
.element {
  transition: background-color $mediumElementDelay linear, color
      $mediumElementDelay linear;
}
```
