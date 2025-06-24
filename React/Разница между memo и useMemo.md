# React

## Разница между `memo` и `useMemo`

`memo` и `useMemo` — это два разных инструмента в React, которые используются для оптимизации производительности компонентов, но они работают по-разному и применяются в разных ситуациях.

### `memo`

`memo` — это функция высшего порядка, которая используется для мемоизации функциональных компонентов. Она предотвращает ненужные повторные рендеры компонента если его `props` не изменились.

В примере ниже `MyComponent` будет рендериться только тогда, когда его `props` изменяются. Если props остаются прежними компонент не будет рендериться повторно, даже если родительский компонент будет перерисован:

```js
import React, { memo } from "react";

const MyComponent = memo(({ name }) => {
  console.log("Rendering MyComponent");

  return <div>Hello, {name}!</div>;
});

function App() {
  const [count, setCount] = React.useState(0);

  return (
    <div>
      <MyComponent name="John" />
      <button onClick={() => setCount(count + 1)}>Increment</button>
      <p>Count: {count}</p>
    </div>
  );
}

export default App;
```

### `useMemo`

`useMemo` — это хук, который используется для мемоизации значений, вычисляемых в функциональных компонентах. Он предотвращает повторные вычисления значений если зависимости не изменились.

В примере ниже `expensiveCalculation` будет выполняться только тогда, когда `count` изменяется. Если `count` не изменяется, `useMemo` вернет ранее вычисленное значение, избегая повторного выполнения тяжелого вычисления:

```js
import React, { useState, useMemo } from "react";

function App() {
  const [count, setCount] = useState(0);
  const [text, setText] = useState("");

  const expensiveCalculation = (num) => {
    console.log("Calculating...");

    for (let i = 0; i < 1000000000; i++) {} // Имитация тяжелого вычисления

    return num * 2;
  };

  const memoizedValue = useMemo(() => expensiveCalculation(count), [count]);

  return (
    <div>
      <input value={text} onChange={(e) => setText(e.target.value)} />
      <button onClick={() => setCount(count + 1)}>Increment</button>
      <p>Count: {count}</p>
      <p>Memoized Value: {memoizedValue}</p>
    </div>
  );
}

export default App;
```

Основные отличия:

- `memo` используется для мемоизации функциональных компонентов. Предотвращает повторные рендеры компонента, если его props не изменились.
- `useMemo` используется для мемоизации значений внутри функциональных компонентов. Предотвращает повторные вычисления значений, если зависимости не изменились.

Оба инструмента помогают оптимизировать производительность приложения, но применяются в разных контекстах. `memo` используется для компонентов, а `useMemo` — для значений и вычислений внутри компонентов.
