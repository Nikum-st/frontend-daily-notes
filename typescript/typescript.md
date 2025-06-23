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
