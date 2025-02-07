# React

## JSX

JSX (JavaScript XML) это синтаксическое расширение для JavaScript, которое используется в React для описания пользовательского интерфейса. По сути является синтаксическим сахаром для функции `React.createElement(component, props, ...children)`. JSX позволяет писать HTML-подобный код внутри JavaScript, что делает код более читаемым и удобным для разработки компонентов интерфейса. При этом важно помнить, что JSX и React это две разные вещи, которые просто можно использовать вместе.

```
return (
  <MyButton color="blue" shadowSize={2}>
    Click Me
  </MyButton>
);
```

Пример выше является JSX выражением, которое будет скомпилировано в итоге в:

```
React.createElement(
  MyButton,
  {
    color: 'blue',
    shadowSize: 2
  },
  'Click Me'
);
```

### Основные особенности JSX

#### Cинтаксис, похожий на HTML

JSX позволяет писать код, который выглядит как HTML, но при этом он полностью интегрирован в JavaScript. Это упрощает создание и чтение компонентов:

```
const element = <h1>Hello, world!</h1>;

```

#### Потомки

В JSX выражении, которое содержит открывающий и закрывающий тег, контент, заключенный между этими тегами, при компиляции передается в специальное свойство: `props.children`, в примере выше это `Click me`. `props.children` может и не быть, а также можно использовать самозакрывающую форму тегов:

```
return (
  <div className="sidebar" />
);
...
React.createElement(
  'div',
  {
    className: 'sidebar'
  },
  null
);
```

В качестве потомков JSX элемента могут выступать другие JSX элементы. Это удобно для отображения вложенных компонентов:

```
return (
  <MyContainer>
    <MyFirstComponent />
    <MySecondComponent />
  </MyContainer>
);
```

Компонент не может возвращать несколько элементов. Код ниже вернёт ошибку `JSX expressions must have one parent`:

```
  return (
    <div>1</div>
    <div>2</div>
    <div>3</div>
  );
```

Если нет корневого элемента верхнего уровня нужно просто обернуть набор элементов в любую обёртку, например элементов `div` или пустым фрагментом (fragment) `<>...</>`:

```
  return (
    <div>
      <div>1</div>
      <div>2</div>
      <div>3</div>
    </div>
  );
```

```
  return (
    <>
      <div>1</div>
      <div>2</div>
      <div>3</div>
    </>
  );
```

Не смотря на то, что JSX выглядит HTML-подобно, под капотом JSX трансформируется в обычные объекты JavaScript.
В JavaScript нельзя напрямую возвращать из функции несколько значений (можно объединять их в объекты или массив), поэтому нельзя возвращать несколько JSX объектов.

#### Отображение falsy значений

Булевые значения (`true`/`false`), `null` и `undefined` игнорируются в JSX. Следующие строки будут выглядеть в итоге одинаково (как пустой `<div></div>`):

```
return (
  <div />
  <div></div>
  <div>{false}</div>
  <div>{true}</div>
  <div>{null}</div>
  <div>{true}</div>
);
```

#### Встраивание JavaScript выражений

Любое JavaScript выражение можно использовать в качестве потомка просто взяв его в фигурные скобки `{...}`:

```
  const foo = "Yay";

  return (
    <MyComponent>{foo}</MyComponent>
  );
```

Эта возможность часто используется для отображения списка JSX выражений произвольной длины:

```
const Item = (props) => {
  return (
    <li>{props.message}</li>;
  )
};

const TodoList = () => {
  const todos = ['complete task', 'drink coffee', 'sleep'];

  return (
    <ul>
      {todos.map((message) => <Item key={message} message={message} />)}
    </ul>
  );
}
```

#### Атрибуты и свойства

В JSX всё должно быть написано в camelCase, например `class` станет `className` (т.к. слово `class` является ключевым зарезервированным словом в JavaScript):

```
<img
  src="https://i.imgur.com/yXOvdOSs.jpg"
  alt="Hedy Lamarr"
  className="photo"
/>
```

По историческим причинам только `aria-_` and `data-_` атрибуты пишутся так же как и в HTML:

```
  return (
    <MyAwesomeComponent
      data-my-id='42'
      aria-label="Yay" />
  );
```
