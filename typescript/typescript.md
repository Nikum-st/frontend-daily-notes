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

#Question 6
Что такое enum в TypeScript и когда его стоит использовать?

##Answer
enum (перечисление) — это способ задать набор именованных констант, которым автоматически присваиваются числовые или строковые значения.
Используется, когда нужно ограничить набор допустимых значений и сделать код более читаемым и безопасным.enum — создаёт объект на уровне JS, доступен в рантайме
Когда применять:
Для ролей пользователей (Admin, User, Guest)
Для фиксированных статусов (Pending, Approved, Rejected)
Для сокращения количества магических строк/чисел в коде

##Example

enum Role {
Admin,
User,
Guest
}

const userRole: Role = Role.Admin;

enum Color {
Red = "RED",
Blue = "BLUE"
}

#Question 7
Что делает оператор as const в TypeScript?

##Answer
Оператор as const преобразует значение в максимально конкретный (литеральный) и неизменяемый тип.

Примитивы становятся литералами: 10, "hello", true вместо number, string, boolean.

Массивы и объекты становятся readonly, а их элементы — литералами.

##Exapmle
const myNumber = 10 as const;
// Тип: 10

const myArray = [1, 2, 3] as const;
// Тип: readonly [1, 2, 3]

const myObject = { a: 1, b: "hi" } as const;
// Тип: { readonly a: 1; readonly b: "hi" }

#Question 8
Что делает Partial<T> utility type в TypeScript?

##Answer
делает все свйоства этого типа необязательными

##Example
interface User {
id: number;
name: string;
email: string;
}

// Теперь все поля необязательны
const update: Partial<User> = {
name: "New Name",
};

#Question 9
Что такое типы объединения (union types) в TypeScript и как их использовать?

##Answer
Они позволяют переменной иметь сразу несколько допустимых типов. Оператор `|` используется для объединения типов.

- Используется, когда значение может быть одного из нескольких типов.
- Особенно полезно при работе с API, параметрами функций, пользовательским вводом.
