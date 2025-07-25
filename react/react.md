##Question 1
Что делает хук useEffect?

##Answer
Выполняет побочные эффекты после рендера: фетчинг, подписка на события.
Например, если прописать в массиве зависимостей, которое идет вторым аргументом, какие-нибудь данные, то при изменении этих данных произойдет перерендер всего компонента.

##Пример
useEffect(()=>{
fetchData()
},[data])

##Question 2
Чем useEffect отличается от useLayoutEffect, и в каких случаях стоит использовать последний?

##Answer
useEffect вызывается после отрисовки и ассинхронно. useLayoutEffect вызывается на этапе инициализации компонента и синхронно, то есть после рендера но до отрисовки на странице.Его нужно использовать в случае, например, если нужно загрузить или инициализоровать конкретные данные до отображения компонента на странице. Или при изменении стиля или позиции до того как пользователь увидит разметку(иначе будет мигание)

##Question 3
Чем отличаются controlled и uncontrolled компоненты?

##Answer
Контролируемые компоненты — это элементы формы, чьё значение управляется через состояние React (useState) и обновляется через onChange. Они используют value, и React полностью контролирует ввод.

Неконтролируемые компоненты управляются самим DOM — данные берутся напрямую через ref, и React не отслеживает каждое изменение.

##Question 4
Что такое hooks? Назови и объясни хотя бы два.

##Answer
Hooks — это функции, добавленные в React для работы с функциональными компонентами. Они позволяют использовать состояние и управлять жизненным циклом без классов.

Существуют:
встроенные хуки, например useState и useEffect
кастомные хуки, которые создаёт сам разработчик для повторного использования логики

useState управляет состоянием компонента.
useEffect выполняет побочные эффекты (side effects) и вызывается после рендера компонента.

##Question 5
Как работает Virtual DOM и зачем он нужен?

##Answer
Virtual DOM — это облегчённая копия реального DOM, которую React использует для оптимизации производительности.

При каждом изменении состояния React сначала обновляет Virtual DOM.
Затем сравнивает его с предыдущей версией (diffing).
И только после этого минимально обновляет реальные изменения в real DOM (reconciliation).

Это делает интерфейс более быстрым и отзывчивым.

##Question 6
Что делает React.memo и в каких случаях его стоит применять?

##Answer
React.memo мемоизирует компонент, предотвращая его повторный рендер при тех же пропсах. Полезен для оптимизации производительности, особенно если компонент тяжёлый или часто получает одни и те же пропсы. Для правильной работы нужно, чтобы передаваемые функции были обёрнуты в useCallback, а объекты — в useMemo.

##Example
const ExpensiveComponent = React.memo(({ value }: { value: number }) => {
console.log("rendered");
return <div>{value}</div>;
});

##Question 7
Что такое "lifting state up" и зачем он используется в React?

##Answer
"Lifting state up" (поднятие состояния) — это перемещение состояния из нескольких дочерних компонентов в их общего родителя. Далее родитель управляет этим состоянием и передаёт его вниз через props.React построен на принципе однонаправленного потока данных (one-way data flow) — данные передаются сверху вниз. Поднятие состояния:

Обеспечивает согласованность (consistency) между связанными компонентами;
Позволяет избежать дублирования состояния в разных местах;
Делает логику управления данными централизованной и предсказуемой;
Упрощает отладку и поддержку.

##Question 8
Что такое key в React и зачем он нужен при рендеринге списков?

##Answer
key — это специальный атрибут, который React использует для отслеживания элементов в списке при повторном рендере. Он помогает эффективно определять, какие элементы были изменены, добавлены или удалены, без полного пересоздания DOM-структуры.

зачем нужен:
Повышает производительность, избегая лишнего перерендера.
Позволяет React сохранять и правильно обновлять состояние компонентов, привязанных к списку.

##Example
{items.map(item => (
<Item key={item.id} value={item.value} />
))}

##Question 9
Что делает хук useMemo в React и зачем он нужен?

##Answer
Хук useMemo используется для мемоизации — он кэширует результат вычислений между рендерами компонента. Это позволяет избежать повторных тяжёлых вычислений, если входные зависимости не изменились.

Когда использовать:
При тяжёлых вычислениях, чтобы избежать повторного выполнения.
При передаче мемоизированных значений в дочерние компоненты, чтобы избежать лишнего рендера.

##Example
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
Значение computeExpensiveValue(a, b) пересчитается только если a или b изменились.

##Question 10
Что такое React.StrictMode и зачем он нужен?

##Answer
<React.StrictMode> — это компонент-обёртка, который активирует дополнительные проверки и предупреждения во время разработки.
Он помогает найти:
-устаревшие API,
-небезопасные жизненные циклы,
-ошибки в useEffect,
-неожиданные побочные эффекты.

Работает только в разработке, не влияет на продакшн.

##Question 11
Как работает reconciliation в React?

##Answer
reconciliation(Согласование) - процесс, который помогает оптимизировать и увеличить производительность, предотвращая лишние перересовки, путем сравнения предыдущего состояния с текущим, после React делает изменение только там, где произошли изменения, а остальной код не трогает. В процессе используется виртуальный DOM.

##Question 12
Что делает хук useCallback и зачем он нужен в React?

##Answer
хук React, который мемоизирует функцию, чтобы она не пересоздавалась при каждом рендере, если не изменились её зависимости. Это помогает избежать лишних рендеров дочерних компонентов и повышает производительность, особенно когда функции передаются через пропсы в компоненты с React.memo.

##Question 13
Что произойдёт, если вы вызовете setState внутри render()?

##Answer
Произойдет бесконечный рендер компонента, так как setState инициирует обнолвения остояние, которое в свою очеред вызывает метод render, и так до бескочности.В результате приложение зависнет, и, возможно, возникнет ошибка "Maximum update depth exceeded" или ошибка переполнения стека

##Question 14
Что делает хук useEffect, если не передать второй аргумент?

##Answer
Если не передать второй аргумент, useEffect выполняется после каждой отрисовки компонента (каждого рендера).
Если передать пустой массив [], эффект выполнится только один раз — после первого рендера.

##Question 15
Что такое пропсы (props) и как они отличаются от состояния (state) в React?

##Answer
Пропсы (props) — это входные параметры компонента, которые передаются ему от родителя и не изменяются внутри компонента. Они используются для передачи данных и функций вниз по иерархии.
Состояние (state) — это внутренние данные компонента, которые он может изменять самостоятельно, чтобы управлять своим поведением и отображением.
Главное отличие: props передаются в компонент сверху и неизменяемы, а state принадлежит самому компоненту и может изменяться внутри него.

##Question 16
Как работает контекст (Context API) в React и в каких случаях его следует использовать вместо пропсов?

##Answer
Context API в React — это механизм для передачи данных по дереву компонентов без необходимости явно передавать пропсы на каждом уровне (избегая props drilling). Он позволяет создать глобальное (для части приложения) хранилище, к которому могут обращаться как родительские, так и глубоко вложенные дочерние компоненты.
Контекст удобно использовать, когда нескольким компонентам нужны одни и те же данные, такие как текущая тема, язык, авторизация и т. д.

##Question 17
Что такое refs в React и когда их стоит использовать вместо состояния (state)?

##Answer
Refs (ссылки) в React используются для доступа напрямую к DOM-элементам или к экземплярам компонентов. Они позволяют обойти обычный поток данных и взаимодействовать с DOM вручную. Это полезно, когда нужно, например, установить фокус на input, измерить размер элемента или управлять воспроизведением видео.

Когда использовать refs вместо state:

Когда нужно взаимодействовать с DOM напрямую
Когда не требуется повторный рендер (в отличие от state)
Для интеграции с нестандартными библиотеками или canvas/видео API

##Question 18
Что такое "фрагменты" (React Fragments) и зачем они нужны?

##Answer
React-фрагменты позволяют обернуть несколько элементов без добавления лишнего DOM-узла. Используются, чтобы избежать ненужных <div> вокруг компонентов. Синтаксис: <Fragment>...</Fragment> или короткий <>...</>.

##Question 19
Что такое JSX и почему он используется в React?

##Answer
SX — это расширение синтаксиса JavaScript, которое позволяет писать разметку, похожую на HTML, прямо внутри кода React-компонентов.
Это упрощает создание UI и делает код более читаемым.
Под капотом JSX компилируется в вызовы React.createElement.
