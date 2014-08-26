## Про объявление функций и о выражениях с ними

Функции в JavaScript являются объектами. Следовательно, их можно передавать и присваивать точно так же, как и любой другой объект. Популярным способом использования этого свойства является передача *анонимной функции* в качестве функции обратного вызова в некую другую функцию — к примеру, при описании асинхронных вызовов.

### Объявление `function`

    // всё просто и привычно
    function foo() {}

В следующем примере, ещё перед запуском всего скрипта, для описанной функции [резервируется](#function.scopes) переменная; за счёт этого она доступна *в любом месте* кода, вне зависимости от того, где она *определена* — даже если она вызывается заранее, перед её фактическим объявлением в коде (и сколь угодно задолго до такого определения).


    foo(); // сработает, т.к. функция будет создана до выполнения кода
    function foo() {}

### `function` как выражение

    var foo = function() {};

В конце примера ниже переменной `foo` присваивается безымянная *анонимная* функция.

    foo; // 'undefined'
    foo(); // вызовет TypeError
    var foo = function() {};

Поскольку выражение с применением `var` *резервирует* имя переменной `foo` ещё до запуска кода, `foo` уже имеет некое значение во время его исполнения (отсутствует ошибка «`foo` is not defined»).

Но поскольку сами *присвоения* исполняются только непосредственно во время работы кода, `foo` по умолчанию будет иметь лишь значение [`undefined`](#core.undefined) (до обработки строки с определением функции).

### Выражения с именованными фунциями

Существует еще один ньюанс, касающийся присваиваний именованных функций:

    var foo = function bar() {
        bar(); // работает
    }
    bar(); // получим ReferenceError

Здесь фукнция `bar` не доступна во внешней области видимости, так как она используется только для присвоения переменной `foo`; однако, внутри `bar` она неожиданно оказывается доступна. Такое поведение связано с особенностью работы JavaScript с [разыменованием](#function.scopes) - имя функции *всегда* доступно в локальной области видимости самой функции.
