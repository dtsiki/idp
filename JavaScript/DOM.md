# JavaScript

Все примеры кода из этого раздела можно найти [🔎 здесь](https://codepen.io/collection/OyyoNd).

## DOM

DOM или Document Object Model — это представление веб-страницы в виде структуры объектов или дерева объектов. Когда браузер загружает HTML создаётся дерево элементов, которое с помощью JavaScript можно модифицировать: изменять, удалять, добавлять новые, etc.

Браузер создаёт DOM при загрузке страницы, складывает его в переменную `document` и сообщает, что DOM создан, с помощью события `DOMContentLoaded`. С переменной `document` начинается любая работа с HTML-разметкой в JavaScript.

![пример DOM](./../assets/images/JavaScript/DOM.jpg)

DOM чаще всего используется в JavaScript, но не является его частью, поэтому иногда с DOM работают в других языках.

### Узлы (Node)

Дерево состоит из обычных и текстовых узлов. Обычные узлы — это HTML-теги, а текстовые узлы — текст внутри тегов.

Обычный узел называется `Element`, и он содержит в себе описание тега, атрибутов тега и обработчиков. Если изменить описание — изменится и HTML-код этого элемента (возможно что-то даже изменится на экране. Например, если поменять цвет шрифта).

У любого узла есть один родительский узел и дочерние. Родительский узел — элемент, в который вложен текущий узел, он может быть только один. Дочерние — узлы, которые вложены в текущий узел.

Это правило не работает только в двух случаях:

1. корневой узел — у такого узла нет родителя;
2. текстовый узел — у таких узлов нет дочерних узлов, только родитель. Последний уровень любого DOM-дерева состоит из текстовых узлов.

Все, что есть в HTML, даже комментарии, является частью DOM. Даже директива `<!DOCTYPE...>`, которая указывается в начале HTML, тоже является DOM-узлом. Она будет находиться в дереве DOM прямо перед тегом `<html>`. Объект `document`, представляющий весь документ, формально тоже является DOM-узлом.

Существует 12 типов узлов, но на практике в основном используются только 4 из них:

1. Document – «входная точка» в DOM
2. узлы-элементы Element – HTML-теги, из которых строится дерево
3. текстовые узлы Text – содержат текст
4. комментарии Comment – иногда в них можно включить информацию, которая не будет показана, но доступна в DOM для чтения JavaScript

### Свойства узлов

HTML-элементы содержат свойства, которые можно разделить на группы:

- свойства, связанные с HTML-атрибутами: ID, классы, стили и так далее
- свойства и методы связанные с обходом DOM: получение дочерних элементов, родителя, соседей
- информация о содержимом
- специфические свойства элемента

#### Свойства, связанные с HTML-атрибутами

К ним относятся:

- `id` - получить идентификатор элемента
- `className` - список классов в HTML-атрибуте class
- `style` - управление стилями (удаление, добавление). Стили добавляются так же с помощью свойств, сами свойства именуются по аналогии с CSS-свойствами

```
<div id="yay" class="text">
  Yay!
</div>

<script>
    const element = document.getElementsByTagName('div')[0];
    console.log(element.className); // Выведет "text"
    console.log(element.id); // Выведет "yay"
    element.style.backgroundColor = 'violet'; // Изменит фон на фиолетовый цвет
</script>
```

[🔎 Codepen](https://codepen.io/thevioletmaniac/pen/GgRrQoj)

#### Свойства и методы, связанные с DOM

К ним относятся:

- `children` — список дочерних элементов
- `parentElement` — получить родительский элемент
- `nextElementSibling` и `previousElementSibling` — получить следующий или предыдущий узел-сосед

```
<div id="yay">
  <ul>
    <li>Element 1</li>
    <li>Element 2</li>
    <li>Element 2</li>
  </ul>
</div>

<script>
    const list = document.getElementsByTagName('ul')[0];
    // children
    console.log(list.children.length); // Выведет 3
    console.log(list.children[0]); // Выведет Element 1
    console.log(list.children[1]); // Выведет Element 2
    console.log(list.children[2]); // Выведет Element 3

    // parentElement
    console.log(list.parentElement.id); // Выведет "yay"

    // nextElementSibling и previousElementSibling
    const second = list.children[1];
    console.log(second.nextElementSibling); // Выведет Element 2
    console.log(second.previousElementSibling); // Выведет Element 1

</script>
```

[🔎 Codepen](https://codepen.io/thevioletmaniac/pen/ZYELrWq)

С помощью этих свойств и методов можно перемещаться по дереву:

- `getElementsByClassName` — поиск среди дочерних элементов по названию класса
- `getElementsByTagName` — поиск среди дочерних элементов по названию тега
- `querySelector` — поиск первого дочернего элемента, подходящего под CSS-селектор
- `querySelectorAll` — поиск всех дочерних элементов подходящих под CSS-селектор

О них рассказано подробно ниже в разделе DOM API

#### Свойства с информацией о содержимом

- `innerHTML` — это свойство возвращает HTML-код всего, что вложено в текущий элемент. При записи в это свойство, предыдущее содержимое будет затёрто. Страница отобразит новое содержимое:

```
<div>Hello, world!</div>
<button onclick="yay()">Yay</button>
<button onclick="qeq()">Qeq</button>

<script>
const div = document.getElementsByTagName('div')[0];
console.log(div.innerHTML); // Выведет "Hello, world!"

function yay() {
    div.innerHTML = '<p>Yay</p>';
}

function qeq() {
    div.innerHTML = '<p>Qeq</p>';
}

</script>
```

- `outerHTML` — это свойство возвращает HTML-код текущего элемента и всего, что в него вложено. При записи в это свойство, предыдущее содержимое будет затёрто.

Свойство `innerHTML` позволяет получить только содержимое элемента как HTML-строку. В то время как `outerHTML` делает то же самое, но при этом возвращает и HTML самого элемента. Можно сказать, что вывод будет идентичен `innerHTML`, только в строке будет содержаться открывающий и закрывающий тег самого элемента, у которого было вызвано свойство.

- `textContent` — свойство, возвращает текст всех вложенных узлов без HTML-тегов

Для считывания и изменения текстового содержимого браузер предоставляет свойства `innerText` и `textContent`. Запись значения работает идентично для обоих. Значение, которое возвращается при чтении свойств, отличается. `textContent` возвращает строку с содержимым всех вложенных потомков, вне зависимости от того, скрыты они или нет. `innerText` же возвращает содержимое только видимых элементов.

Свойство может изменить только текстовое содержимое элемента. Если присвоить строку, содержащую HTML, то она вставится как простой текст и не превратится в реальный DOM-элемент. Для того чтобы вставлять HTML c помощью строки, подойдёт свойство innerHTML.

```
<div>
  Qeq
  <div>Yay</div>
</div>

<input type="text" id="input" value="<h1>Hello, world!</h1>">
<button onclick="change()">Change</button>

<script>
const div = document.getElementsByTagName('div')[0];
console.log(div.outerHTML); // Выведет полностью изначальный div из HTML-разметки
console.log(div.textContent); // Выведет две строки: 1) Qeq 2) Yay

function change() {
    const input = document.getElementsByTagName("input")[0];
    div.outerHTML = input.value;
}
</script>
```

[🔎 Codepen](https://codepen.io/thevioletmaniac/pen/raNjJjP)

### Как работает DOM?

1. Браузер читает HTML и создает дерево DOM, где каждый тег — это узел.
2. Скрипты JavaScript могут изменять DOM: добавлять, удалять, обновлять элементы.
3. Изменение DOM триггерит перерисовку страницы что может быть медленным если обновлений много.

Пример работы с DOM в JavaScript - находим элемент с индетификатором id="myText" и изменяем его содержимое при нажатии кнопки:

```
document.getElementById("myButton").addEventListener("click", function() {
  document.getElementById("myText").innerText = "Текст изменен!";
});
```

### DOM API

DOM предоставляет API для работы с элементами. Ниже представлен краткий список распространённых методов API, используемых в для работы с DOM в JavaScript:

- `getElementById`
- `getElementsByTagName`
- `getElementsByClassName`
- `querySelector`
- `querySelectorAll`
- `createElement`
- `appendChild`, `append`

#### Поиск элемента по его идентификатору `getElementById`

`getElementById` - метод JavaScript, который позволяет получить элемент на веб-странице по его уникальному атрибуту `id`. Он используется для того, чтобы получить ссылку на элемент и работать с ним в JavaScript.

`getElementById` имеет следующий синтаксис, где `id` это строка, представляющая уникальный идентификатор элемента:

```
document.getElementById(id);
```

Если элемент найден, метод возвращает ссылку на этот элемент `HTMLElement`, который затем может быть использован для изменения его свойств или добавления/удаления обработчиков событий. Метод вернёт `null` если ничего не нашлось.

##### Примеры использования `getElementById`

1. Замена текста по нажатию на кнопку

```
<p id="text">Hello, world!</p>
<button onclick="changeText()">Change</button>

<script>
    function changeText() {
        document.getElementById("text").innerText = "Yay!";
    }
</>
```

[🔎 Codepen](https://codepen.io/thevioletmaniac/pen/raNjYOV)

2. Скрытие элемента по нажатию на кнопку

```
<p id="text">Yay!</p>
<button onclick="hideText()">Hide</button>
<button onclick="showText()">Show</button>

<script>
    function hideText() {
        document.getElementById("text").style.display = "none";
    }

    function showText() {
        document.getElementById("text").style.display = "block";
    }
</script>
```

[🔎 Codepen](https://codepen.io/thevioletmaniac/pen/azbpVdZ)

3. Управлением значением поля ввода

```
<input type="text" id="input" value="Hello, world!">
<button onclick="changeInput()">Change</button>

<script>
    function changeInput() {
        document.getElementById("input").value = "Yay!";
    }
</script>
```

[🔎 Codepen](https://codepen.io/thevioletmaniac/pen/pvoRdgX)

4. Добавление новых элементов
   Помимо модификации уже существующих элементов с помощью `getElementById` можно добавлять новые элементы. В примере ниже каждый клик добавляет новый элемент `<li>` в список:

```
<ul id="list">
  <li>Element 1</li>
  <li>Element 2</li>
</ul>
<button onclick="addItem()">Add</button>

<script>
    function addItem() {
        let newItem = document.createElement("li");
        newItem.innerText = "New element";
        document.getElementById("list").appendChild(newItem);
    }
</script>

```

[🔎 Codepen](https://codepen.io/thevioletmaniac/pen/JojEOXE)

#### Поиск элементов по заданному тегу `getElementsByTagName`

Метод `getElementsByTagName` определён для объекта document и любого HTML-элемента (`Element`) страницы. Позволяет найти все элементы с заданным тегом среди дочерних. Возвращает похожую на массив `HTMLCollection` с найденными элементами. Если элементов не нашлось, то коллекция будет пустая, то есть с размером 0.

Отличается от `getElementById` тем, что находит все элементы с указанным тегом (`div`, `p`, `button` и т. д.).

##### Примеры использования

1. Замена текста по всех выбранных тегах, например `<p>`:

```
<p>First</p>
<p>Second</p>
<p>Third</p>
<button onclick="changeText()">Change</button>

<script>
    function changeText() {
        let paragraphs = document.getElementsByTagName("p");

        for (let i = 0; i < paragraphs.length; i++) {
            paragraphs[i].innerText = "Yay " + (i + 1);
        }
    }
</script>
```

[🔎 Codepen](https://codepen.io/thevioletmaniac/pen/mydRqwK)

Аналогично можно измененить цвета всех элементов `<p>`:

```
function changeColor() {
    let items = document.getElementsByTagName("p");

    for (let i = 0; i < items.length; i++) {
        items[i].style.color = "purple";
    }
}
```

2. Изменение всех кнопок на странице:

```
<button>Button 1</button>
<button>Button 2</button>
<button>Button 3</button>
<button onclick="disableButtons()">Disable them all</button>

<script>
    function disableButtons() {
        let buttons = document.getElementsByTagName("button");

        for (let i = 0; i < buttons.length; i++) {
            buttons[i].disabled = true;
        }
    }
</script>

```

[🔎 Codepen](https://codepen.io/thevioletmaniac/pen/jEOyaLg)

3. Подсчет количества элементов

```
<p>Text 1</p>
<p>Text 2</p>
<p>Text 3</p>
<button onclick="countParagraphs()">Count</button>
<p id="result"></p>

<script>
  function countParagraphs() {
    let paragraphs = document.getElementsByTagName("p");

    document.getElementById("result").innerText = "Items: " + paragraphs.length;
  }
</script>
```

[🔎 Codepen](https://codepen.io/thevioletmaniac/pen/azbpVLJ)

#### Поиск элементов по названию класса `getElementsByClassName`

Метод `getElementsByClassName` находит все элементы с указанным классом и возвращает `HTMLCollection` (массивоподобный объект).

Оба метода `getElementsByClassName` и расмотренный выше `getElementsByTagName` возвращают коллекцию элементов, поэтому с ними удобно работать в цикле.

`getElementsByClassName` метод принимает один параметр — название класса или список классов в виде строки:

```
const oneClass = document.getElementsByClassName('just-class');
const multiplyClass = document.getElementsByClassName('first-class second-class third-class');
```

##### Примеры использования `getElementsByClassName`

1. Изменение текста всех элементов только с определенным классом

В примере ниже по нажатию на кнопку измениться текст только у первого и последнего параграфа с текстом:

```
<p class="text">Text 1</p>
<p class="not-text">Text 2</p>
<p class="text">Text 3</p>

<button onclick="changeText()">Change</button>

<script>
    function changeText() {
        let items = document.getElementsByClassName("text");

        for (let i = 0; i < items.length; i++) {
            items[i].innerText = "Next text " + (i + 1);
        }
    }
</script>

```

[🔎 Codepen](https://codepen.io/thevioletmaniac/pen/VYwPQrj)

2. Добавление нового класса ко всем элементам

```
<p class="text">Text 1</p>
<p class="text">Text 2</p>
<p class="text">Text 3</p>
<button onclick="addNewClass()">Add a new class</button>

<style>
    .awesome {
        font-weight: bold;
        color: violet;
    }
</style>

<script>
    function addNewClass() {
        let items = document.getElementsByClassName("text");

        for (let i = 0; i < items.length; i++) {
            items[i].classList.add("awesome");
        }
    }
</script>
```

[🔎 Codepen](https://codepen.io/thevioletmaniac/pen/KwKaQQx)

#### Получение DOM-элемента по CSS-селектору `querySelector`

Метод `querySelector` определён для объекта `document` и любого HTML-элемента `Element` страницы. Позволяет найти элемент по CSS-селектору среди дочерних. Если элементов несколько, то вернётся _первый_ подходящий. Если подходящих элементов нет, то вернёт `null`.

Метод принимает один параметр — CSS-селектор в виде строки: `.class`, `#id`, любой HTML-тег, вложенность HTML-тегов и т. д. Если передан не CSS-селектор, то система выбросит ошибку.

```
const awesomeText = document.querySelector(".awesome-text");
const ul = document.querySelector("ul");
const firstParagraph = document.querySelector("div > p");
```

Далее можно делать всё тоже самое, что делали в примерах выше.

```
<ul>
    <li class="element">Element 1</li>
    <li class="awesome-element">Element 2</li>
    <li class="awesome-element">Element 3</li>
    <li class="element">Element 4</li>
</ul>

<button onclick="changeText()">Change text</button>
<button onclick="hideFirst()">Hide first</button>

<script>
    function changeText() {
        document.querySelector(".element").innerText = "Updated element!";
    }

    function hideFirst() {
        document.querySelector("ul li").style.display = "none";
    }
</script>
```

[🔎 Codepen](https://codepen.io/thevioletmaniac/pen/yyLgvqZ)

По сравнению с другими методами `querySelector` более гибкий, так как позволяет искать по разным селекторам, а не только по идентификатору. Если нужно несколько элементов можно использовать `querySelectorAll`, который работает работает как `getElementsByClassName`, но возвращает `NodeList`.

#### Получение всех DOM-элементов по CSS-селектору `querySelectorAll`

Метод `querySelectorAll`(selector) ищет все элементы, соответствующие CSS-селектору, и возвращает `NodeList` (массивоподобный объект). Работает аналогично `querySelector`, но возвращает все найденные элементы поэтому результат можно `NodeList` можно перебирать через `forEach` и прочие методы перебора.

Далее можно делать всё тоже самое, что делали в примерах выше.

```
<ul>
    <li class="element">Element 1</li>
    <li class="awesome-element">Element 2</li>
    <li class="awesome-element">Element 3</li>
    <li class="element">Element 4</li>
</ul>

<button onclick="changeAll()">Change all</button>
<button onclick="changeOnlyAwesome()">Change only awesome</button>

<script>
    function changeAll() {
        let elements = document.querySelectorAll("ul li");

        elements.forEach((item, index) => {
            item.innerText = "Awesome text " + (index + 1);
            item.style.color = "violet";
        });
    }

    function changeAwesome() {
        let elements = document.querySelectorAll(".awesome-element");

        elements.forEach((item, index) => {
            item.innerText = "Awesome text " + (index + 1);
            item.style.outline = "1px solid violet";
        });
    }
</script>
```

[🔎 Codepen](https://codepen.io/thevioletmaniac/pen/yyLgvqZ)

#### Создание элементов `createElement` и добавление элементов `appendChild`/`append`

Метод `createElement` создает динамически новые HTML-элементы. После этого можно устанавливать созданному элементу текст, атрибуты, стили:

```
createElement(localName, options);
```

Метод `createElement` создает элементы в памяти, но не добавляет их в DOM, для этого их нужно добавлять в DOM вручную через `appendChild` или `append`. Оба метода добавляют элемент внутрь родителя, но у них есть различия.

Метод `appendChild` добавляет только один узел `Node`. При этом возвращает добавленный элемент и не позволяет добавлять текст напрямую:

```
parent.appendChild(child);
```

Метод `append` позволяет добавлять несколько элементов и строки в конец узла `Node`. При этом не возвращает добавленный элемент, но позволяет вставлять текст:

```
parent.append(child1, child2, ..., childN);
```

##### Примеры использования

1. Добавление нового параграфа в `div`
   При каждом нажатии в `div` добавляется новый параграф `<p>`:

```
<div id="container"></div>
<button onclick="addParagraph()">Add</button>

<script>
    function addParagraph() {
        let newParagraph = document.createElement("p");
        newParagraph.innerText = "New parapgraph!";
        document.getElementById("container").appendChild(newParagraph);
    }
</script>
```

[🔎 Codepen](https://codepen.io/thevioletmaniac/pen/emYgbor)

2.  Создание кнопки и добавление обработчика события

При нажатии создается новая кнопка, которая при клике вызывает `alert`:

```
<div id="container"></div>
<button onclick="createButton()">Create a button</button>

<script>
    function createButton() {
        let newButton = document.createElement("button");
        newButton.innerText = "Click me!";

        newButton.onclick = function() {
            alert("Button clicked");
        };

        document.getElementById("container").appendChild(newButton);
    }
</script>
```

[🔎 Codepen](https://codepen.io/thevioletmaniac/pen/raNjogr)

3. Создание списка `<ul>` с элементами `<li>`

По нажатию на кнопку добавляется новый элемент в список:

```
<ul id="list">
  <li>Default list item</li>
</ul>
<button onclick="addListItem()">Add a list item</button>


<script>
    function addListItem() {
        let item = document.createElement("li");
        item.innerText = "New list item";
        document.getElementById("list").appendChild(item);
    }
</script>
```

[🔎 Codepen](https://codepen.io/thevioletmaniac/pen/WbNRLqO)

4. Создание ссылки `<a>` с атрибутами

```
<div id="links"></div>
<button onclick="addLink()">Add a link</button>

<script>
    function addLink() {
        let link = document.createElement("a");
        link.innerText = "Go to Google";
        link.href = "https://google.com";
        link.target = "_blank";
        document.getElementById("links").appendChild(link);
    }
</script>
```

[🔎 Codepen](https://codepen.io/thevioletmaniac/pen/GgRrPVo)

5. Создание нескольких элементов

```
<div id="container"></div>
<button onclick="createCard()">Create a card</button>

<style>
    .card {
        border: 1px solid violet;
        padding: 10px;
        margin: 5px;
        border-radius: 5px;
    }
</style>

<script>
    function createCard() {
        let card = document.createElement("div");
        card.classList.add("card");

        let title = document.createElement("h3");
        title.innerText = "Title";

        let text = document.createElement("p");
        text.innerText = "Description";

        card.appendChild(title);
        card.appendChild(text);
        document.getElementById("container").appendChild(card);
    }
</script>
```

[🔎 Codepen](https://codepen.io/thevioletmaniac/pen/MYWJZNV)

6. Добавление текста и элемента сразу `append`

```
<div id="container"></div>
<button onclick="appendText()">append()</button>
<button onclick="appendChildText()">appendChild()</button>


<script>
    function appendText() {
        let span = document.createElement("span");
        span.innerText = " this is a span text ";

        document.getElementById("container").append("Hello,", span, " How are you?");
    }

    /* Вернёт ошибку "TypeError: Node.appendChild: At least 1 argument required, but only 0 passed"
    function appendChildText() {
        let span = document.createElement("span");
        span.innerText = " this is a span text ";

        document.getElementById("container").appendChild("Hello,", span, " How are you?");
    }
    */

    function appendChildText() {
        let span = document.createElement("span");
        span.innerText = " this is a span text ";

        document.getElementById("container").appendChild(span);
    }
</script>
```

[🔎 Codepen](https://codepen.io/thevioletmaniac/pen/qEBRgWG)

7. Добавление нескольких элементов с `append`

```
<ul id="list"></ul>
<button onclick="addItems()">Add items</button>

<script>
    function addItems() {
        let item1 = document.createElement("li");
        item1.innerText = "Item 1";

        let item2 = document.createElement("li");
        item2.innerText = "Item 2";

        document.getElementById("list").append(item1, item2);
    }
</script>
```

[🔎 Codepen](https://codepen.io/thevioletmaniac/pen/vEYgbOO)

8. Разница в возврате значений append appendChild

```
<script>
    let div = document.createElement("div");
    let span = document.createElement("span");

    console.log(div.appendChild(span)); // Вернёт <span></span>
    console.log(div.append(span)); // Вернёт undefined
</script>
```

[🔎 Codepen](https://codepen.io/thevioletmaniac/pen/ZYELwGj)

### Недостатки DOM

- Изменение элемента обновляет всю страницу (даже если поменялась только одна строка текста).
- Частые изменения вызывают «перерисовку» (Reflow & Repaint), замедляя интерфейс.
- Большие приложения становятся медленными из-за дорогих операций с DOM.

Эти недостатки решаются Virtual DOM. Virtual DOM уменьшает количество изменений в реальном DOM, обновляя только нужные части. Это делает работу интерфейса быстрее и плавнее. Подробнее об Virtual DOM можно посмотреть в разделе React/Virtual DOM.

## Источники

1. [Document Object Model (DOM)](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model)
2. [DOM](https://doka.guide/js/dom/)
