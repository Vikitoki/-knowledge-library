# Особенности реализации библиотеки

## Словарь терминов React

Ссылка на источник: <https://ru.reactjs.org/docs/glossary.html>

## Однонаправленный поток данных

В иерархии компонентов ни родительский, ни дочерние компоненты не знают, задано ли состояние другого компонента.
Также не важно, как был создан определённый компонент — с помощью функции или с помощью класса.

Состояние часто называют «локальным», «внутренним» или инкапсулированным.
Оно доступно только для самого компонента и скрыто от других.

Компонент может передать своё состояние вниз по дереву в виде пропсов дочерних компонентов.
Это, в общем, называется «нисходящим» («top-down») или «однонаправленным» («unidirectional») потоком данных.
Состояние всегда принадлежит определённому компоненту, а любые производные этого состояния могут влиять только на компоненты,
находящиеся «ниже» в дереве компонентов.

## React key

Ключи должны быть уникальными среди соседних элементов.
Ключи внутри массива должны быть уникальными только среди своих соседних элементов.
Им не нужно быть уникальными глобально. Можно использовать один и тот же ключ в двух разных массивах.
Пример:

```jsx
function Blog(props) {
  const sidebar = (
    <ul>
      {props.posts.map((post) => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  );
  const content = props.posts.map((post) => (
    <div key={post.id}>
      <h3>{post.title}</h3>
      <p>{post.content}</p>
    </div>
  ));
  return (
    <div>
      {sidebar}
      <hr />
      {content}
    </div>
  );
}

const posts = [
  {
    id: 1,
    title: "Привет, мир",
    content: "Добро пожаловать в документацию React!",
  },
  { id: 2, title: "Установка", content: "React можно установить из npm." },
];
ReactDOM.render(<Blog posts={posts} />, document.getElementById("root"));
```

### React предохранители

Если какой-то модуль не загружается (например, из-за сбоя сети), это вызовет ошибку.
Вы можете обрабатывать эти ошибки для улучшения пользовательского опыта с помощью Предохранителей.
После создания предохранителя, его можно использовать в любом месте над ленивыми компонентами для отображения состояния ошибки.

Классовый компонент является предохранителем,
если он включает хотя бы один из следующих методов жизненного цикла:

- static getDerivedStateFromError()
- componentDidCatch().

Используйте `static getDerivedStateFromError()` при рендеринге запасного UI в случае отлова ошибки.
Используйте `componentDidCatch()` при написании кода для журналирования информации об отловленной ошибке.
Обратите внимание, что предохранители отлавливают ошибки **исключительно в своих дочерних компонентах**.

Начиная с React 16, ошибки, не отловленные ни одним из предохранителей,
будут приводить к размонтированию всего дерева компонентов React.

```jsx
import React, { Suspense } from "react";
import ErrorBoundary from "./ErrorBoundary";

const OtherComponent = React.lazy(() => import("./OtherComponent"));
const AnotherComponent = React.lazy(() => import("./AnotherComponent"));

const MyComponent = () => (
  <div>
    <ErrorBoundary>
      <Suspense fallback={<div>Загрузка...</div>}>
        <section>
          <OtherComponent />
          <AnotherComponent />
        </section>
      </Suspense>
    </ErrorBoundary>
  </div>
);
```