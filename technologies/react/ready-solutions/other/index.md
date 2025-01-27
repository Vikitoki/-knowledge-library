# Прочее

## Предохранитель Error Boundary

```jsx
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error) {
    // Обновить состояние с тем, чтобы следующий рендер показал запасной UI.
    return { hasError: true };
  }

  componentDidCatch(error, errorInfo) {
    // Можно также сохранить информацию об ошибке в соответствующую службу журнала ошибок
    logErrorToMyService(error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      // Можно отрендерить запасной UI произвольного вида
      return <h1>Что-то пошло не так.</h1>;
    }

    return this.props.children;
  }
}
```

Можно воспользоваться уже готовым решением, по типу [react-error-boundary](https://www.npmjs.com/package/react-error-boundary)

## Виртуализация длинных списков

Если ваше приложение рендерит длинные списки данных (сотни или тысячи строк),
мы рекомендуем использовать метод известный как «оконный доступ».

react-window и react-virtualized — это популярные библиотеки для оконного доступа.
Они предоставляют несколько повторно используемых компонентов для отображения списков, сеток и табличных данных.

Ссылки на примеры:

- <https://react-window.vercel.app/#/examples/list/fixed-size>
- <https://bvaughn.github.io/react-virtualized/>
