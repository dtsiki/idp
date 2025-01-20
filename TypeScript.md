# TypeScript

[TOC]

## Базовые типы

Для установки типа применяется знак двоеточия, после которого указывается название типа:

```
let num: number = 42;
let hello: string = 'Yay';
let isAwesome: boolean = false;
```

Есть два способа преобразования типов (type assertion):

```
const myCanvas = document.getElementById("main_canvas") as HTMLCanvasElement;
```

```
const myCanvas = <HTMLCanvasElement>document.getElementById("main_canvas");
```

---

### number

— используется для хранения числовых данных. Это могут быть как целые числа так и числа с плавающей запятой:

```
const age: number = 33;
const price: number = 99.99;
```

---

### string

— представляет текстовые строки, то есть последовательности символов, такие как слова, фразы и символы. Можно использовать шаблонные строки (template literals):

```
const name: string = 'Daria';
const message: string = `Hello, ${name}`;
```

---

### boolean

— логическое значение, которое может принимать одно из двух значений - true или false:

```
let isAwesome: boolean = true;
let isHappy: boolean = false;
```

---

### array

— массивы или упорядоченные списки элементов. Элементами могут быть любые другие типы:

```
const numbers: number[] = [1, 2, 3, 4, 5];

const colors: string[] = ['red', 'green', 'blue'];

const users: Array<{id: number; name: string}> = [
  {id: 1, name: 'Daria'},
  {id: 2, name: 'Anton'},
];
```

---

### symbol

— уникальные и неизменяемые значения, которые используются для создания уникальных идентификаторов и свойств объектов. Каждое значение типа Symbol уникально и не может быть равным другому Symbol:

```
const firstName = Symbol("name");
const secondName = Symbol("name");
```

---

### bigInt

— less common primitives (c), для представления целых больших чисел:

```
const oneHundred: bigint = BigInt(100);
const anotherHundred: bigint = 100n;
```

---

### null и undefined

— принимают соответствующие значения `undefined` и `null` как и в JavaScript. `null` представляет явное отсутствие значения и обычно используется когда значение переменной специально установлено в "ничто" или "пусто". `undefined` указывает на то, что переменной не было присвоено значения и она не инициализирована.

---

### void

— отсутствие конкретного типа, противоположность `any` т.е. означает отсутствие конкретного типа. Используется для указания на отсутствие значения или возвращаемого результата функции. `void` часто применяется к функциям, которые не возвращают какое-либо значение, или к переменным, которые не имеют определенного значения:

```
function sayHi(): void {
  console.log('Hello!');
}

function returnSomethingNotVoid(): void {
  let result: number = 42;

  return result; // Error
}
```

---

### enum

— тип данных, который позволяет создавать именованные наборы констант, например числовые:

```
enum Direction {
  Up = 1,
  Down,
  Left,
  Right,
}
```

Перечисления могут быть не только числовыми, но и строковыми, а также быть инициализированы значениями:

```
enum Direction {
  Up = "UP",
  Down = "DOWN",
  Left = "LEFT",
  Right = "RIGHT",
}
```

Можно использовать неоднородные перечисления, но лучше не надо:

```
enum BooleanLikeHeterogeneousEnum {
  No = 0,
  Yes = "YES",
}
```

---

### any

— динамический тип данных, который может представлять любое значение. Тип `any` является уникальным для TypeScript, в JavaScript подобного типа не существует.

```
let myVar: any = 42;
myVar = 'Yay';
myVar = true;

const doSomething = (data: any) => {
  console.log(data);
};

doSomething(42);
doSomething('Hello');
doSomething({ name: 'Kesha', age: 3 });
```

Если при объявлении переменных и полей не было присвоено значение, компилятором будет выведен тип данных `any`:

```
var someVar; // someVar: any
let someLet; // someVar: any

class SomeClass {
  name; // name: any
}
```

Если функция возвращает значение, принадлежащее к типу, который компилятор не в состоянии вывести, то возвращаемый этой функцией тип данных будет также выведен как тип `any`:

```
function sum(a, b) {
  // function sum(a: any, b: any): any
  return a + b;
}
```

Хотя использование `any` предоставляет гибкость, рекомендуется избегать его в большинстве случаев, поскольку это может снизить надежность и читаемость кода. Предпочтительнее использовать явные типы данных, чтобы TypeScript мог обеспечивать статическую проверку и предупреждения о возможных ошибках:

```
const convertToUpperCase = (a: any) => {
  return a.toUpperCase();
};

const result = multiply(42); // TypeError: a.toUpperCase is not a function
```

> В наиболее строгом режиме с помощью `strict: true` в `tsconfig.json` использование `any` невозможно.

---

### unknown

Бывают ситуации когда тип неизвестен, но работа с ним должна быть безопасна с точки зрения типов. Для этого в TypeScript существует дополнение к `any` — `unknown`. Отличается от `any` тем, что над `unknown` запрещено выполнение каких-либо операций:

```
let myAnyVar: any;
myAnyVar.a = 5;
myAnyVar.a = "";
myAnyVar();

const myUnknownVar: unknown = myAnyVar; // Тут будет всё в порядке
myUnknownVar.a = 5; // 'myUnknownVar' is of type 'unknown'
myUnknownVar.a = ''; // 'myUnknownVar' is of type 'unknown'
myUnknownVar(); // 'myUnknownVar' is of type 'unknown'
```

Тип `unknown` позволяется использовать только в операциях равенства ===, ==, !== и != и в операциях с логическими операторами &&, || и !:

```
  const myUnknownVar0: unknown = 42;

  const myUnknownVar1 = 42 === myUnknownVar0; // Ok
  let myUnknownVar2 = 42 !== myUnknownVar0; // Ok

  let myUnknownVar3 = 42 > myUnknownVar0; // Error: 'myUnknownVar0' is of type 'unknown'
  let myUnknownVar4 = 42 < myUnknownVar0; // Error: 'myUnknownVar0' is of type 'unknown'
  let myUnknownVar5 = 42 >= myUnknownVar0; // Error: 'myUnknownVar0' is of type 'unknown'
  let myUnknownVar6 = 42 <= myUnknownVar0; // Error: 'myUnknownVar0' is of type 'unknown'
  let myUnknownVar7 = 42 - myUnknownVar0; // Error: 'myUnknownVar0' is of type 'unknown'
  let myUnknownVar8 = 42 * myUnknownVar0; // Error: 'myUnknownVar0' is of type 'unknown'
  let myUnknownVar9 = 42 / myUnknownVar0; // Error: 'myUnknownVar0' is of type 'unknown'
  let myUnknownVar10 = ++myUnknownVar0; // Error: 'myUnknownVar0' is of type 'unknown'
  let myUnknownVar11 = --myUnknownVar0; // Error: 'myUnknownVar0' is of type 'unknown'
  let myUnknownVar12 = myUnknownVar0++; // Error: 'myUnknownVar0' is of type 'unknown'
  let myUnknownVar13 = myUnknownVar0--; // Error: 'myUnknownVar0' is of type 'unknown'

  let myUnknownVar14 = 42 && myUnknownVar0; // Ok
  let myUnknownVar15 = 42 || myUnknownVar0; // Ok
  let myUnknownVar16 = myUnknownVar0 || 42; // Ok
  let myUnknownVar17 = !myUnknownVar0; // Ok

```

### never

— тип, который указывает на ситуацию, в которой функция не завершает выполнение или выражение не возвращает никакого значения. Этот тип обычно используется для функций, которые генерируют ошибки, бесконечных циклов или других сценариев, при которых выполнение программы невозможно завершить нормально:

```
// Генерация ошибки
function throwError(message: string): never {
  throw new Error(message);
}

// Бесконечный цикл
function loop(): never {
  while (true) {
    // Бесконечный цикл
  }
}

```

Ещё один вариант использования `never` - в функциях, в которых передаётся какой-либо параметр, но не используется:

```
const doSomething(_: never, somethingImportant: TSomethingImportant) {
    useSomethingImportant(somethingImportant);
};

```

---

## Типы и интерфейсы (type vs interface)

Помимо базовых типов TypeScript предоставляет разработчикам операторы для определения интерфейсов `interface` и псевдонимов для типов `type`. Во многих случаях `interface` и `type` взаимозаменяемы.

Типы используются для задания именованных типов данных, включая примитивы, объекты, функции и массивы. Они позволяют объединять или пересекать типы и поддерживают использование ключевых слов `typeof`, `keyof` при присвоении.

Интерфейсы служат для описания структуры объектов. Интерфейсы поддерживают декларативное объединение и могут быть расширены другими интерфейсами или классами.

И типы, и интерфейсы позволяют описывать структуры данных в TypeScript, что помогает предотвратить ошибки на этапе компиляции и делать код более предсказуемым.

### Типы

Оператор `type` позволяет определить новый псевдоним для существующего типа. Слово «псевдоним» используется неслучайно - оператор `type` фактически добавляет дополнительное имя для существующего типа, новый тип данных при этом не создаётся:

```
type TAwesomeString = string;
const myAwesomeString: TAwesomeString = 'Yay';
```

Фактически это дополнительное имя для типа `string`. При объявлении переменной типа `TAwesomeString`, нам не придётся делать каких-то преобразований при попытке записать в неё строковое значение, ведь `TAwesomeString` — дополнительное имя для типа `string`.

Другие примеры использования типов:

```
export type TMode = "multiply" | "screen" | "none";

export type TLabelPosition = "left" | "right" | "center" | null;

export type TTextAlign = "left" | "right" | "center";

export type TFontSettings = {
  family: string;
  size: number;
  weight?: number | string;
};

export type TLoadingGetter = () => boolean;

export type TMountHandler = () => void;

export type TFormFields = {
  [Authentication.FormFieldName.NAME]: string;
  [Authentication.FormFieldName.LAST_NAME]: string;
  [Authentication.FormFieldName.IS_ADMIN]: boolean;
};
```

---

### Интерфейсы

Аналог типов. Интерфейсы определяются с помощью ключевого слова `interface`:

```
interface IAwesomeString {
  title: string;
}

const myAwesomeString: IAwesomeString = {
  title: "Yay",
};
```

Интерфейс определяет свойства и методы, которые объект должен реализовать. Другими словами, интерфейс - это определение кастомного типа данных, но без реализации:

```
interface IUser {
  id: number;
  name: string;
}

let employee: IUser = {
  id: 42,
  name: "Daria"
}
```

При определении интерфейса можно задать некоторые свойства как необязательные с помощью знака вопроса. Подобные свойства реализовать необязательно:

```
interface IUser {
  id: number;
  name: string;
  age?: number;
}

let employee: IUser = {
  id: 1,
  name: "Alice",
  age: 23
}

let manager: IUser = {
  id: 2,
  name: "Bob"
}
```

Также интерфейс может содержать свойства только для чтения, значение которых нельзя изменять. Такие свойства определяются с помощью ключевого слова `readonly`:

```
interface IPoint {
  readonly x: number;
  readonly y: number;
}

let point: IPoint = { x: 10, y: 20 };
console.log(point.x); // ????? тут должна быть ошибка, но оно работает???
```

Интерфейсы удобны при использовании объектно-ориентированного подхода. Сначала проектируется интерфейс, а потом классы, которые его имплементируют. Для этого в TypeScript есть отдельная синтаксическая конструкция `implements`:

```
interface ICat {
  meow: () => void;
}

class Tiger implements ICat {
  meow() {
    console.log('Meow!');
  }
}
```

#### Разница между типами и интерфейсами

Разницы почти нет, всё что можно сделать с помощью типов можно сделать с помощью интерфейсов и наоборот. Но есть небольшие отличия.

##### Расширение

Интерфейсы и типы можно расширять. Расширение интерфейсов:

```
interface IAnimal {
  name: string;
}

interface IBear extends IAnimal {
  honey: boolean;
}

const bear = getBear();
bear.name;
bear.honey;
```

Расширение типов:

```
type TAnimal = {
  name: string;
}

type TBear = TAnimal & {
  honey: boolean;
}

const bear = getBear();
bear.name;
bear.honey;
```

##### Добавление новых полей

Интерфейсам можно добавлять новые поля:

```
interface IWindow {
  title: string;
}

interface IWindow {
  ts: TypeScriptAPI;
}

const src = 'const a = "Hello World"';
window.ts.transpileModule(src, {});
```

Типа нельзя добавлять новые поля и модицифировать их после создания:

```
type TWindow = {
  title: string;
}

type TWindow = {
  ts: TypeScriptAPI;
}

// Error: Duplicate identifier 'Window'.
```

---

### Объединения и пересечения типов (Union/Intersection)

— способы замены наследования в TypeScript.

#### Объединение

Объединение позволяет создавать новые типы данных из существующих.
Объединение указывается с помощью оператора прямой черты `|`, по обе стороны которой располагаются типы данных, которые будут объеденены:

```
let TNewType: TType1 | TType2 | TType3;
```

Объединение обозначает что значение, которому присваивается тип, созданный с помощью объединения типов может принадлежать только одному типу, например `number` или `string`:

```
function printId(id: number | string) {
  console.log("Your ID is: " + id);
}

printId(42); // Ok
printId("42"); // Ok
printId({ myID: 22342 }); // Error: Argument of type '{ myID: number; }' is not assignable to parameter of type 'string | number'.
```

TypeScript разрещает только операции, которые будут валидны для каждого объединяемого типа:

```
function printId(id: number | string) {
  console.log(id.toUpperCase()); // Error: Property 'toUpperCase' does not exist on type 'string | number'. Property 'toUpperCase' does not exist on type 'number'.
}
```

#### Пересечение

Пересечение указывается с помощью оператора амперсанда `&`, по обе стороны которого указываются типы данных.

```
let TNewType: TType1 & TType2 & TType3;
```

В этом случае значение должно обладать всеми обязательными признаками каждого типа, определяющего пересечение:

```
type TUser = {
  username: string,
};

type TAdmin = {
  isAdmin: boolean,
};

type TUserAdmin = TUser & TAdmin;
```

Пересечения удобно использовать для расширения каких-либо типов данных:

```
interface IResponseInit {
  headers?: HeadersInit;
  status?: number;
  statusText?: string;
}

let repsonse: IResponseInit & { url: string };
```

### Дженерики

— обобщённые типы (Generic types).
Если мы напишем функцию и жёстко зададим тип, то она сможет работать только со значениями этого типа. Значения других типов передать не получится:

```
function doSomething(arg: number): number {
  return arg;
}
```

Можно исправить это написав под каждый тип отдельную функцию:

```
function doSomething(arg: string): string {
  return arg;
}
```

```
function doSomething(arg: boolean): boolean {
  return arg;
}
```

Можно просто использовать `any`:

```
function doSomething(arg: any): any {
  return arg;
}
```

С помощью дженериков можно писать код, который одинаково будет работать со значениями разных типов:

```
function doSomething<Type>(arg: Type): Type {
  return arg;
}

let somethingString = doSomething<string>("Yay");
let somethingNumber = doSomething<number>(42);
let somethingBoolean = doSomething<boolean>(true);

```

---

## Источники

[Типы данных в TypeScript | PurpleScroll](https://purpleschool.ru/knowledge-base/article/types)
[Everyday Types | TypeScript Doc](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html)
[Enums | TypeScript Doc](https://www.typescriptlang.org/docs/handbook/enums.html)
[Интерфейсы и типы TypeScript: в чём разница | Доктайп](https://htmlacademy.ru/blog/js/types-vs-interfaces)
[Типы или интерфейсы в TypeScript: что и когда использовать? | Хабр](https://habr.com/ru/articles/844990/)
[Generics | TypeScript Doc](https://www.typescriptlang.org/docs/handbook/2/generics.html)
[Для чего использовать дженерики в TypeScript | Доктайп](https://htmlacademy.ru/blog/js/typescript-generic)
[Как правильно использовать тип any | PurpleScroll](https://purpleschool.ru/knowledge-base/article/any)
[Примитивные типы Null, Undefined, Void, Never, Unknown | Справочник Typescript](https://scriptdev.ru/guide/014/)
