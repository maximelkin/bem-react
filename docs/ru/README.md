# Bem React

Набор утилит для разработки пользовательских интерфейсов по [БЭМ-методологии](https://ru.bem.info) на [React](https://github.com/facebook/react). Bem React поддерживает аннотации типов [TypeScript](http://www.typescriptlang.org) и [Flow](https://flow.org).

## Описание концепции

Основая мысль — работаем через полную композицию. Работу с CSS-классами и модификаторы выражаем через [HOCи](https://reactjs.org/docs/higher-order-components.html), а переопределение кода по платформам и экспериментам через DI.

### Необходимое от БЭМ в React
- декларативное разделение кода по [модификаторам](https://ru.bem.info/methodology/key-concepts/#Модификатор) и [уровням переопределения](https://ru.bem.info/methodology/redefinition-levels/) (платформам, экспериментам);
- [работа с CSS-классами](https://ru.bem.info/methodology/naming-convention/).

### Состав пакетов
- [`@bem-react/classname`](./packages/classname/README.md)
    - `cn(block, elem?): { mods(object?): string; toString(): string; }`
- [`@bem-react/core`](./packages/core/README.md)
    - `withBemMod(cn, predicate, HOC)(Component)`
- [`@bem-react/di`](./packages/di/README.md)
    - `Registry`: `Map<string, React.ComponentType>`
    - `withRegistry(registry)(Component)`

Модификатор (`withBemMod`) более не сможет влиять на внутреннее устройство компонента. Дополнительная функциональность должна быть выражена через управляющие компоненты сверху. А сами компоненты могут быть выражены как `React.ComponentType` по необходимости без каких-либо базовых БЭМ-компонентов. Подключение модификаторов не отличается от подключения любых других HOC и работает через любой compose.

Работа с CSS-классами состоит из трех этапов:
- статическое создание базового CSS-класса сущности;
- динамическое добавление CSS-классов модификаторов;
- динамическое добавление CSS-классов миксов.

Статическое создание CSS-классов выражается через функцию-хелпер (`@bem-react/classname`).

Динамическое добавление CSS-классов выражается через HOC (`withBemMod`) модификатора с единственным требованием к внешнему интерфейсу компонента в виде опционального пропса `className`. Модификатор определит истинность предиката и добавит дополнительный класс через пропсы к базовому классу через ту же функцию-хелпер, что используется и для статических CSS-классов.

Переопределение компонентов и их составляющих выражается через dependency injection (DI) (`@bem-react/di`), который реализуется на базе [`React.ContextAPI`](https://reactjs.org/docs/context.html) и множественных реестров (`Registry`) компонентов. Каждый компонент волен регистрировать свои зависимости в реестре и позволять переопределять их сверху, что напрямую не заложено в работу контекста, но реализуется способом резолвинга во время провайдинга нового значения контекста. По умолчанию зависимости возможно переопределять и по контексту вниз, что есть стандартный механизм работы контекста. В итоге, DI — это HOC, который провайдит реестры в контекст. Реестры могут работать в режимах проваливания и всплытия зависимостей. Это позволяет переопределять что угодно, где угодно, на любом уровне вложенности, простым дописыванием в нужный реестр.

## Внести вклад

Bem React является библиотекой с открытым исходным кодом, которая находится в стадии активной разработки, а также используется внутри компании [Яндекс](https://yandex.ru/company/).

Если у вас есть предложения по улучшению API, вы можете прислать [Pull Request](https://github.com/bem/bem-react-core/pulls).

Если вы нашли ошибку, вы можете создать [issue](https://github.com/bem/bem-react-core/issues) с описанием проблемы.

Подробное руководство по внесению изменений см. в [CONTRIBUTING.md](CONTRIBUTING.md).

## Лицензия

© 2018 [Яндекс](https://yandex.ru/company/). Код выпущен под [Mozilla Public License 2.0](LICENSE.txt).