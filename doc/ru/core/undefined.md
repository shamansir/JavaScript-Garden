## `undefined` и `null`

В JavaScript используется два отдельных типа для описания *ничего* — `null` и `undefined`, при этом более полезным из них оказывается `undefined`.

### Значение `undefined`

`undefined` — это тип с единственным возможным значением: `undefined`.

Кроме этого, в языке определена глобальная переменная со значением `undefined`, причём эта переменная так и называется — `undefined`. Не являясь константой, она не является и ключевым словом. Из этого следует, что её значение можно с лёгкостью переопределить.

> **Замечание по ES5:** в ECMAScript 5 переменная `undefined` **больше не** *доступна на запись* в strict-режиме, однако она всё ещё может быть перегружена по имени, например — функцией с именем `undefined`.

Список случаев, когда код возвращает значение `undefined`:

 - При попытке доступа к глобальной переменной `undefined` (если она не была переопределена).
 - При попытке доступа к переменной, которая *ещё не была* инициализирована каким-либо значением.
 - Неявный возврат из функции при отсутствии в ней оператора `return`.
 - Из оператора `return`, который не возвращает явного значения.
 - В результате поиска несуществующего свойства у объекта (и/или доступа к нему).
 - При попытке доступа к аргументу функции, который не был передан в неё явно.
 - При попытке доступа к чему-либо, чьим значением является `undefined`.
 - В результате вычисления любого выражения, соответствующего форме `void(выражение)`.

### Защита от потенциальных изменений значения `undefined`

Поскольку глобальная переменная `undefined` содержит копию реального *значения* `undefined`, присвоение этой переменной нового значения **не** изменяет значения у *типа* `undefined`.

Получается, чтобы проверить что-либо с *типом* `undefined` (на соответствие *значению* `undefined`), прежде нужно узнать изначальное значение самой *переменной* `undefined`.

Для того, чтобы защитить код от случайного переопределения переменной `undefined`, часто используют технику [анонимной обёртки](#function.scopes), в которую добавляют аргумент и намеренно не передают его значение.

    var undefined = 123;
    (function(something, foo, undefined) {
        // в локальной области видимости `undefined`
        // снова ссылается на правильное значене.

    })('Hello World', 42);

Другой способ достичь того же эффекта — использовать определение внутри обёртки.

    var undefined = 123;
    (function(something, foo) {
        var undefined;
        ...

    })('Hello World', 42);

Единственная разница между этими вариантами в том, что последняя версия при минификации будет занимать больше на 4 байта, поскольку в первом случае внутри анонимной обёртки нет дополнительного оператора `var`.

### Применение `null`

Хотя `undefined` в контексте языка JavaScript чаще используется в роли традиционного *null*, настоящий `null` (и тип и литерал) является, в какой-то степени, просто другим типом данных.

Он используется во внутренних механизмах JavaScript (в случаях вроде установки конца цепочки прототипов, через присваивание `Foo.prototype = null`). Но практически во всех случаях тип `null` может быть заменён на равносильный ему `undefined`.
