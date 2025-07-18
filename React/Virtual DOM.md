# React

## Virtual DOM

Virtual DOM (виртуальный DOM) — это концепция, используемая в современных JavaScript-библиотеках и фреймворках, таких как React, для оптимизации обновления пользовательского интерфейса.

Virtual DOM представляет собой абстракцию над реальным DOM (Document Object Model) и позволяет более эффективно управлять изменениями в интерфейсе. DOM (Document Object Model) — это программный интерфейс для HTML и XML документов. Он представляет документ в виде дерева узлов, где каждый узел соответствует части документа (например, элементу, атрибуту или тексту). Браузеры используют DOM для рендеринга веб-страниц и предоставляют API для манипуляции этим деревом.

Проблемы с обычным DOM:

1. Медленные операции: Манипуляции с DOM могут быть медленными, особенно если требуется часто обновлять большое количество элементов. Каждое изменение в DOM может вызывать перерисовку и перерасчет стилей, что может негативно сказываться на производительности.
2. Сложность управления состоянием: При работе с большим количеством элементов и сложными состояниями управление обновлениями DOM может стать сложным и запутанным.

Virtual DOM — это легковесное представление реального DOM, которое хранится в памяти и используется для оптимизации обновлений пользовательского интерфейса. Вместо того чтобы напрямую изменять реальный DOM, изменения сначала применяются к виртуальному DOM. Затем библиотека или фреймворк сравнивает виртуальный DOM с предыдущей версией (процесс, называемый диффинг или diffing) и вычисляет минимальный набор изменений, необходимых для синхронизации виртуального DOM с реальным DOM.

Преимущества Virtual DOM:

1. Повышенная производительность: Поскольку изменения сначала применяются к виртуальному DOM, а затем минимальный набор изменений применяется к реальному DOM, это снижает количество операций с реальным DOM и улучшает производительность.
2. Упрощенное управление состоянием: Virtual DOM позволяет более эффективно управлять состоянием и обновлениями интерфейса, что упрощает разработку сложных приложений.

### Этапы

1. Создание виртуального DOM: При первом рендеринге компонента создается виртуальный DOM, который представляет структуру интерфейса.
2. Обновление состояния: Когда состояние компонента изменяется, создается новая версия виртуального DOM.
3. Сравнение (диффинг): Новая версия виртуального DOM сравнивается с предыдущей версией, чтобы определить, какие части интерфейса изменились.
4. Применение изменений: Минимальный набор изменений применяется к реальному DOM, чтобы синхронизировать его с виртуальным DOM.

```
import React, { useState } from 'react';
import ReactDOM from 'react-dom';

function Counter() {
  const [count, setCount] = useState(0);
  return (
    <div>
        <p>Count: {count}</p>
        <button onClick={() => setCount(count + 1)}>
            Increment
        </button>
    </div>
  );
}

ReactDOM.render(<Counter />, document.getElementById('root'));
```

В этом примере React использует Virtual DOM для управления обновлениями интерфейса. Когда состояние компонента `Counter` изменяется (например, при нажатии кнопки), React создает новую версию виртуального DOM, сравнивает ее с предыдущей версией и применяет минимальные изменения к реальному DOM.

## Как работает Virtual DOM?

Процесс обновления DOM дерева проходит в 3 этапа:

1. Создание Virtual DOM

Когда React-компонент рендерится, он создаёт виртуальное представление DOM (объект JavaScript).

```
const virtualDOM = {
  type: "div",
  props: {
    className: "container",
    children: [
      { type: "h1", props: { children: "Hello, Virtual DOM!" } },
      { type: "p", props: { children: "Update without rerenders!" } }
    ]
  }
};
```

Это просто обычный объект в памяти, а не настоящий HTML.

2. Сравнение Virtual DOM (Diffing)

При обновлении React создаёт новый Virtual DOM и сравнивает его со старым. Этот процесс называется diffing (разница между состояниями).

React сравнивает:

- Какие элементы удалились.
- Какие изменились.
- Какие остались неизменными.

Пример сравнения Virtual DOM:

```
// Old Virtual DOM:
const oldVDOM = { type: "h1", props: { children: "Hello!" } };

// New Virtual DOM:
const newVDOM = { type: "h1", props: { children: "Hello, world!" } };

```

React понимает, что поменялся только текст и обновит только этот элемент, а не весь h1.

3. Обновление реального DOM (Reconciliation)
   После diffing React применяет изменения к настоящему DOM минимальными операциями.

Если меняется только текст – он обновляет только текст. Если изменился класс – он обновляет только класс. Другие элементы остаются нетронутыми.
