##Question 1
Что такое interface и чем он отличается от type alias?

##Answer
interface и type в TypeScript используются для описания типов. Основное отличие:

interface — предназначен для описания структуры объектов, особенно в ООП-стиле. Задает какие свойста и методы должны содержать объекты
Поддерживает:

наследование (extends)
объединение (declaration merging)
может быть расширен повторно

type — более гибкий, может описывать объединения, пересечения, примитивы, кортежи и другие сложные типы.

Оба не компилируются в JavaScript — служат только для проверки типов на этапе разработки.
##Example
interface User {
login: string;
name: string;
age: number;
}

interface Address {
city: string;
street: string;
apartment?: number; // необязательное свойство
}

interface InfoUser extends User, Address {
isAdmin: boolean;
}

const user: InfoUser = {
login: "nikitaST",
name: "Nikita",
age: 23,
city: "Moscow",
street: "Novaya",
isAdmin: true,
};

##Question 2
Что такое Generics и зачем они нужны?

##Answer
Generics — это возможность создавать обобщённые (универсальные) и типобезопасные функции, интерфейсы и классы. Они позволяют работать с разными типами данных без потери информации о типе.

##Example
без дженериков

function create (args: any): any {
return args
}
Недостаток: теряется информация о типе, нет подсказок от TypeScript.

с дженериком

function create <T> (args: T): T {
return args
}

теперь возможно переиспользовать эту функцию с разными типами

const createdData = create<number>(23)
const createdData = create<string>("Hellow")

##Question 3
Объясни разницу между any, unknown и never.

##Answer
any – отключает проверку типов. Можно присвоить любое значение и выполнять любые операции без ошибок от компилятора.
unknown – безопасная альтернатива any. Требует проверки типа перед использованием.
never – указывает, что функция никогда не возвращает значение, например, при бесконечном цикле или исключении (throw).

##Examples
function clearedData(data: any): any {
const cleared = data;
return cleared;
}

function clearedDataSafe(data: unknown): unknown {
if (data === null) {
throw new Error("Введите данные");
}

if (typeof data === "string") {
return "";
}

    return data;

}

function throwError(text: string): never {
throw new Error(text);
}

##Question 4
Что такое type guard в TypeScript и зачем он нужен?

##Answer
Type guard (защитник типов) — это механизм, позволяющий уточнить, к какому типу принадлежит значение в определённом контексте. Он используется для логической проверки типов во время выполнения, чтобы компилятор понимал, с каким именно типом работает код. Это особенно важно при работе с объединёнными (union) или неизвестными (unknown) типами — например, string | number.
`typeof`: для примитивов (`string`, `number`, `boolean`, `symbol`, `bigint`, `undefined`, `function`)
`instanceof`: для классов и объектов
Пользовательские функции с `is` : например, `function isString(value: unknown): value is string`

##Example
// typeof
function handle(value: string | number) {
if (typeof value === 'string') {
console.log(value.toUpperCase()); // TypeScript знает, что это string
} else {
console.log(value.toFixed(2)); // А здесь — number
}
}

// instanceof
class Animal {
makeSound() {}
}
class Dog extends Animal {
bark() {}
}

function makeNoise(animal: Animal) {
if (animal instanceof Dog) {
animal.bark(); // TypeScript знает, что это Dog
}
}

// Пользовательская функция
function isString(value: unknown): value is string {
return typeof value === 'string';
}

function process(value: unknown) {
if (isString(value)) {
console.log(value.toUpperCase());
}
}

##Question 5
Что такое readonly в TypeScript и чем отличается от const?

##Answer
readonly — это модификатор, применяемый к свойствам объектов, чтобы запретить их изменение после инициализации.Readonly<T> — это утилитный тип, делающий все свойства объекта T только для чтения.

const — это ключевое слово, которое делает переменную неизменной (нельзя переназначить ссылку), но не защищает содержимое объекта.

Отличие:
const защищает переменную, но не её внутреннее состояние.
readonly защищает свойства объекта.

##Example

type house {
city: string,
size: number
}

const myHouse: Readonly<house> = {
city: New-York,
size: 100
}

myHouse.city = Moscow //❌ Ошибка: нельзя изменить свойство, так как оно readonly
