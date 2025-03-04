# JavaScript

## Контекст `this`

В JavaScript `this` — это ключевое слово, которое указывает на контекст выполнения функции.

`this` — это специальная переменная, которая ссылается на объект, в контексте которого была вызвана функция.

Значение `this` зависит от того, как и где функция была вызвана.

### Глобальный контекст

В глобальной области видимости (вне функций) `this` ссылается на глобальный объект:

1. `window` в браузере
2. `global` в Node.js

```
console.log(this); // Выведет Window (в браузере)
```

![console.log(this); вызвано в консоли гугла](./../assets/images/JavaScript/Google%20this.png)

### Контекст в функции

В обычной функции `this` ссылается на глобальный объект в нестрогом режиме или `undefined` в строгом режиме:

```
function showThis() {
    console.log(this);
}

showThis(); // Выведет Window (в браузере, нестрогий режим)
```

### Контекст в методах объекта

Если функция вызывается как метод объекта, `this` ссылается на этот объект:

```
const obj = {
    name: "Bob",
    sayHi() {
        console.log(`Hello, my name is ${this.name}`);
    }
};

obj.sayHi(); // Выведет "Hello, my name is Bob"
```

### Контекст в конструкторах

Если функция вызывается с `new` (как конструктор), `this` ссылается на новый созданный объект:

```
function Person(name) {
    this.name = name;
}

const person = new Person("Bob");
console.log(person.name); // Выведет "Bob"
``

### Контекст в стрелочных функциях
```

В стрелочных функциях `this` берется из окружающего контекста (лексический контекст):

```
const obj = {
    name: "Bob",
    sayHi: () => {
        console.log(`Hello, my name is ${this.name}`);
    }
};

obj.sayHi(); // Выведет "Hello, my name is undefined" т.к. this берется из внешнего контекста
```

## Передача контекста

Иногда нужно явно указать, какой объект должен быть контекстом для функции. Для этого используются методы:

### call

`call` вызывает функцию с указанным контекстом и аргументами:

```
function sayHy() {
    console.log(`Hello, my name is ${this.name}`);
}

const person = { name: "Bob" };
sayHy.call(person); // Выведет "Hello, my name is Bob"
```

### apply

Работает как `call`, но принимает аргументы в виде массива:

```
function sayHy(greeting) {
    console.log(`${greeting}, my name is ${this.name}`);
}

const person = { name: "Bob" };
sayHy.apply(person, ["Hi"]); // Выведет "Hi, my name is Bob"
```

### bind

Создает новую функцию с привязанным контекстом. Функция не вызывается сразу, а возвращается:

```
function sayHy() {
    console.log(`Hello, my name is ${this.name}`);
}

const person = { name: "Bob" };
const boundGreet = sayHy.bind(person);
boundGreet(); // Выведет "Hello, my name is Bob"
```

### Примеры передачи контекста

Пример использования `call`:

```
const car1 = { brand: "Toyota" };
const car2 = { brand: "Honda" };

function showBrand() {
    console.log(`This car is a ${this.brand}`);
}

showBrand.call(car1); // This car is a Toyota
showBrand.call(car2); // This car is a Honda
```

Пример использования `apply`:

```
function introduce(greeting, punctuation) {
    console.log(`${greeting}, my name is ${this.name}${punctuation}`);
}

const person = { name: "Alice" };
introduce.apply(person, ["Hi", "!"]); // Hi, my name is Alice!
```

Пример использования `bind`:

```
const person = { name: "Bob" };

function greet() {
    console.log(`Hello, my name is ${this.name}`);
}

const boundGreet = greet.bind(person);
boundGreet(); // Hello, my name is Bob
```

## Контекст в обработчиках событий

Часто контекст теряется в обработчиках событий. Решение — использовать `bind`:

```
const button = document.querySelector("button");

const obj = {
    name: "Bob",
    handleClick() {
        console.log(`Button clicked by ${this.name}`);
    }
};

// Без bind контекст будет потерян
button.addEventListener("click", obj.handleClick); // Button clicked by undefined

// С bind контекст сохраняется
button.addEventListener("click", obj.handleClick.bind(obj)); // Button clicked by Alice
```

## Контекст в стрелочных функциях

Стрелочные функции не имеют своего `this`, они берут его из окружающего контекста.

```
const obj = {
    name: "Alice",
    greet: function() {
        setTimeout(() => {
            console.log(`Hello, my name is ${this.name}`);
        }, 1000);
    }
};

obj.greet(); // Hello, my name is Alice (через 1 секунду)
```
