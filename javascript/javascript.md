##Question 1
Что произойдёт, если сравнить [] == ![] и почему?

##Answer
Будет true. Так как в данном случаем происходит нестрогое сравнение объекта с булевым значением, сначала Js переводит массив в примитив(string).Получается "" == false. Потом он переводит два операнда в один тип данных. false == false или 0 == 0

##Question 2
В чём разница между var, let и const?

##Answer
var, let и const используются для объявления переменных, но различаются по области видимости, возможности переопределения и поведению при hoisting'е:

var:
Имеет функциональную область видимости
Подвержен hoisting'у (переменная поднимается и инициализируется undefined)
Можно переопределять и переназначать

let:
Блочная область видимости
Подвержен hoisting'у, но не инициализируется — использовать до объявления нельзя
Можно переназначать, но нельзя переобъявить в одной области

const:
Блочная область видимости
Нельзя переназначить (значение фиксируется)
Объекты можно мутировать, но не переназначать саму ссылку

##Question 3
Что такое замыкание (closure) в JavaScript?

##Answer
Closure (замыкание) — это функция, которая «помнит» переменные из своей внешней (лексической) области видимости, даже если эта функция была вызвана вне этой области.
Это позволяет внутренней функции доступ к переменным родительской функции, даже после завершения выполнения родителя.В React функциональные компоненты — это замыкания: они «запоминают» значения из области, в которой были объявлены.

##Exemple
function showSum(a: number): void {
const b = 10;

function createSum(): number {
return a + b; // доступ к переменным из внешней области
}

console.log(createSum());
}

##Question 4
Чем отличаются == и ===? Когда использовать каждый?

##Answer
Оператор == выполняет нестрогое сравнение — он приводит операнды к одному типу перед сравнением.
Оператор === выполняет строгое сравнение — сравнивает и тип, и значение, без преобразования типов.

Рекомендуется всегда использовать ===, чтобы избежать неожиданных результатов при неявном преобразовании типов.

##Example
0 == '0' // true — нестрогое сравнение, строка '0' преобразуется в число
0 === '0' // false — разные типы (number vs string)

[] == ![] // true — [] приводится к "", а ![] к false, потом оба к 0
[] === ![] // false — разные типы

##Question 5
Что произойдёт при выполнении следующего кода и почему?
console.log([] + {});  
console.log({} + []);

##Answer
"[object Object]"
0

[] + {} → пустой массив приводится к "", объект к "[object Object]", результат: "" + "[object Object]" = "[object Object]".
{} + [] → если строка начинается с {}, интерпретатор JS может воспринять это как блок кода, а не объект. Поэтому:
{} интерпретируется как пустой блок (ничего не делает).
+[] приводит пустой массив к числу: +[] = 0.

##Question 6
Что такое "this" в JavaScript и как контекст выполнения влияет на его значение?

##Answer
this — это специальное ключевое слово, которое указывает на объект, к которому применяется текущий код, и его значение зависит от контекста выполнения функции.

Как работает this в разных контекстах:

1. Глобальный контекст
   В браузере: this === window
   В Node.js: this === global, но в модулях this === {}

2. Метод объекта
   Если функция вызывается как метод: obj.method(), то this указывает на obj.

3. Функция (обычная)
   При вызове обычной функции this будет:

undefined в strict mode;
глобальный объект в non-strict mode.

4. Стрелочная функция
   У стрелочной функции нет собственного this — она наследует его из внешнего лексического контекста (то, где она была определена).
5. Конструкторы и классы
   Внутри конструктора или метода класса this указывает на создаваемый или текущий экземпляр объекта.

6. Явное управление this
   С помощью .call(), .apply() и .bind() можно вручную задать значение this.

##Example
function logYear() {
console.log(this.year);
}

const car = {
name: "Corolla",
year: 1998,
logYear: logYear
};

logYear(); // undefined (в глобальном контексте нет year)
car.logYear(); // 1998 (this указывает на car)

const arrowFn = () => console.log(this);
arrowFn(); // берёт this из внешнего контекста (например, window в браузере)

##Question 7
Что делает метод bind() в JavaScript и когда он используется?

##Answer
Метод bind() создаёт новую функцию, у которой жёстко задан контекст (this) и, при необходимости, частично применённые аргументы.
В отличие от call() и apply(), bind() не вызывает функцию сразу, а возвращает новую — которую можно вызвать позже.

##Example

function greet() {
console.log(`Hello, ${this.name}`);
}

const user = { name: "Alice" };

const greetUser = greet.bind(user); // создаём новую функцию с this = user
greetUser(); // Hello, Alice

##Question 8
В чём разница между null и undefined?

##Answer
null - это явное отсутствие значение у перменной, она инициализирована как "пустая". В отличии от underfined(неопределенный) - переменная объявлена, но значенич не присвоено

##Example
let a;
console.log(a); // undefined

let b = null;
console.log(b); // null

##Question 9
Чем отличается event.preventDefault() от event.stopPropagation()?

##Answer
Обе функции используются для управления поведением событий в JavaScript, но делают разное. event.preventDefault() — отменяет поведение по умолчанию, например, отправку формы или переход по ссылке. а stopPrepagation останавивает распростраения события по DOM дереву, которое по умолчанию стоит "всплытие", то есть от элемента по которому было произведено событие и до глобального элемента windows

##Example
form.addEventListener('submit', function(e) {
e.preventDefault(); // форма не отправляется
});

button.addEventListener('click', function(e) {
e.stopPropagation(); // событие не "всплывает" выше
});
