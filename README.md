# Содержание

 - [Изучение Концепций Блокчейна](#изучение-концепций-блокчейна)
 - [Язык программирования Cadence](#язык-программирования-cadence)
   - [Основные сведения](#основные-сведения)
   - [Переменные](#переменные)
   - [Функции](#функции)
   - [Массивы](#массивы)
   - [Словари](#словари)
   - [Опционалы](#опционалы)
   - [Структуры](#структуры)
   - [Ресурсы](#ресурсы)
   - [Ссылки](#ссылки)
   - [Интерфейсы](#интерфейсы)
   - [Контроль доступа](#контроль-доступа)
   - [Пред/пост условия](#предпост-условия)
   - [События](#события)
   - [Скрипты](#скрипты)
   - [Транзакции](#транзакции)
   - [Аккаунты](#аккаунты)
   - [Хранилище аккаунта](#хранилище-аккаунта)
   - [Контракты](#контракты)
 - [Практика](#практика)
   - [Квесты](#квесты)
   - [Создаём собственный NFT смарт-контракт](#создаём-собственный-nft-смарт-контракт)
 - [Литература](#литература)

### Порядок прохождения курса

Глава 1: [Изучение Концепций Блокчейна](#изучение-концепций-блокчейна) -> [Язык программирования Cadence](#язык-программирования-cadence)

Глава 2: [Основные сведения](#основные-сведения) -> [Переменные](#переменные) & [Функции](#функции) & [Контракты](#контракты) -> [Скрипты](#скрипты) & [Транзакции](#транзакции) & [Аккаунты](#аккаунты) -> [Массивы](#массивы) & [Словари](#словари) & [Опционалы](#опционалы) -> [Структуры](#структуры)

Глава 3: [Ресурсы](#ресурсы) -> [Ссылки](#ссылки) -> [Интерфейсы](#интерфейсы) -> [Контроль доступа](#контроль-доступа)

Глава 4: [Хранилище аккаунта](#хранилище-аккаунта) ([Раздел `/storage/`](#раздел-storage)) -> [Хранилище аккаунта](#хранилище-аккаунта) ([Разделы `/public/` и `/private/`](#разделы-public-и-private))

Глава 5: [Пред/пост условия](#предпост-условия) & [События](#события)

# Изучение Концепций Блокчейна

**Блокчейн** - это распределённая последовательная цепочка блоков. Каждый блок содержит информацию о транзакциях, собственную хеш-сумму, а также хеш-сумму предыдущего блока. Таким образом, изменить какую-либо информацию в блокчейне практически невозможно.

**Смарт-контракт** - неизменяемый объект, который живёт в блокчейне и имеет набор функций и данных.
 - Данные в смарт-контракте возможно изменить путем вызова соответствующих функций. *Требуется комиссия.*
 - Данные в смарт-контракте возможно прочитать. *Бесплатно.*

# Язык программирования Cadence

## Основные сведения

**Cadence** - язык программирования смарт-контрактов для блокчейна Flow.

Основные принципы языка:
 - **Безопасность**: Позволяет разработчикам меньше ошибаться благодаря сильной статической системе типов, а также предотвращает угрозы со стороны злоумышленников.
 - **Чистота кода**: Позволяет легко читать и понимать код, что дает дополнительные гарантии безопасности для пользователей контракта.
 - **Простота освоения**: Позволяет новым разработчикам быстро вливаться в процесс благодаря знакомому синтаксису языка и богатой документации.
 - **Опыт разработки**: Позволяет меньше тратить времени на отлов ошибок и больше уделять времени на написание логики.
 - **Ресурсно-ориентированное программирование**

## Переменные

```swift
[модификатор доступа] [var/let] [имя переменной]: [тип переменной]
```

 - `var` используется для изменяемых переменных, а `let` для констант.
 - При инициализации допустимо не указывать тип.

## Функции

```swift
[модификатор доступа] fun [имя функции](параметры): [возвращаемый тип] { ... }
```

Параметры представляют из себя список:
```swift
параметр1: Тип, параметр2: Тип, ...
```

## Массивы

```swift
var array: [Type] = [element1, element2, ... ] // объявление

log(array[0]) // обращение по индексу
```

Полезные методы:
 - `append(_ element: Type)` - добавление элемента в конец.
 - `contains(_ element_: Type): Bool` - проверяет, содержит ли массив элемент.
 - `remove(at: Int)` - удаляет элемент с индексом `at`.
 - `length(): Int` - возвращает длину массива.

## Словари

```swift
var dict: [FromType: ToType] = {key1: value1, key2: value2, ... } // объявление

log(dict[key1]) // обращение по ключу
```

❗ При обращении по ключу возвращает *опционал*.

Полезные методы:
 - `insert(key: Type, _ value: Type)` - добавить пару `key: value`.
 - `remove(key: Type): Type?` - удалить значение по ключу и вернуть его.
 - `keys: [Type]` - массив ключей.
 - `values: [Type]` - массив значений.

## Опционалы

Представляет из себя механизм обработки исключительных ситуаций, например, использование неправильного ключа в словаре.

```swift
var someVar: Type? // опционал
```

Дополняет множество значений типа `Type` значением `nil`.

### Force-unwrap оператор

Используется для "разворачивания" опционала в исходный тип (Type? -> Type).

```swift
var dict: [FromType: ToType] = { ... }
var value: ToType = dict[someKey]! // Force-Unwrap
```

Если опционал имел значение `nil` то произойдёт ошибка.

## Структуры

Набор данных, объединённых по какому-либо смыслу.

```swift
// объявление
pub struct StructName {
    pub let someVar1: String
    pub let someVar2: Int
    ...

    init(...) { ... }
}

// использование
var strct = StructName(...)
log(strct.someVar1)
```

❗ Структуры могут иметь только модификатор доступа `pub`.

## Ресурсы

Похожи на структуры но с некоторыми ограничениями:

 - Нельзя копировать, можно перемещать с помощью оператора `<-`.
 - Создаются только при использовании ключевого слова `create`.
 - Уничтожаются только при использовании ключевого слова `destroy`.

Эти ограничения позволяют жёстко регулировать количество экземпляров какого-либо ресурса, что несёт в себе большую безопасность. Например, это не даст создать случайно копию уникальной NFT.

```swift
pub contract Test {
    pub resource Greeting {
        pub let message: String
        init() {
            self.message = "Hello, Mars!"
        }
    }

    pub fun createGreeting(): @Greeting {
        let myGreeting <- create Greeting()
        return <- myGreeting
    }
}
```

Тип ресура имеет префикс `@`. Например, типом ресурса  `pub resource Jacob {...}` является  `@Jakob`.

❗ Ресурсы объявляются и создаются только внутри контракта.

### Ресурсы в массивах

```swift
pub contract Test {
    // объявление ресурса
    pub resource Greeting {
        pub let message: String
        init() {
            self.message = "Hello, Mars!"
        }
    }

    // объявление массива ресурсов
    pub var arrayOfGreetings: @[Greeting]

    // добавление в массив
    pub fun addGreeting(greeting: @Greeting) {
        self.arrayOfGreetings.append(<- greeting)
    }

    // извлечение из массива
    pub fun removeGreeting(index: Int): @Greeting {
        return <- self.arrayOfGreetings.remove(at: index)
    }

    init() {
        // инициализация пустого массива ресурсов
        self.arrayOfGreetings <- []
    }
}
```

### Ресурсы в словарях

```swift
pub contract Test {
    // объявление ресурса
    pub resource Greeting {
        pub let message: String
        init() {
            self.message = "Hello, Mars!"
        }
    }

    // объявление словаря, содержащего ресурсы
    pub var dictionaryOfGreetings: @{String: Greeting}

    // добавление в словарь | вариант 1
    pub fun addGreeting(greeting: @Greeting) {
        let key = greeting.message
        self.dictionaryOfGreetings[key] <-! greeting
    }

    // добавление в словарь | вариант 2
    pub fun addGreeting(greeting: @Greeting) {
        let key = greeting.message
        let oldGreeting <- self.dictionaryOfGreetings[key] <- greeting
        destroy oldGreeting
    }

    // извлечение из словаря
    pub fun removeGreeting(key: String): @Greeting {
        let greeting <- self.dictionaryOfGreetings.remove(key: key)
            ?? panic("Could not find the greeting!")
        // или используя оператор force-unwrap
        let greeting <- self.dictionaryOfGreetings.remove(key: key)!
        return <- greeting
    }

    init() {
        // инициализация пустого словаря ресурсов
        self.dictionaryOfGreetings <- {}
    }
}
```

В первом варианте добавления используется оператор принудительного перемещения `<-!`.<br>Работает он так: если до добавления в словаре не было пары соответствующей ключу `key`, то по этому ключу будет записано значение, иначе произойдет ошибка.

А во втором варианте добавления используется оператор смещения `<- target <-`. В данном случае значение, которое могло потенциально находиться в словаре, перемещается в переменную `oldGreeting`, а затем туда записывается новое значение `greeting`.

## Ссылки

Ссылки - механизм обращения и обработки данных.

Ссылки позволяют определить идентификатор (переменную), который будет ссылаться на существующие данные, например, на поле структуры или ресурса. Это позволит не перемещать ресурс, а орудовать только ссылкой на интересующие данные.

```swift
pub contract Test {
    // объявление ресурса
    pub resource Greeting {
        pub let language: String
        init(_language: String) {
            self.language = _language
        }
    }

    // словарь ресурсов
    pub var dictionaryOfGreetings: @{String: Greeting}

    // получение ссылки на элемент словаря
    pub fun getReference(key: String): &Greeting {
        return &self.dictionaryOfGreetings[key] as &Greeting
    }

    init() {
        // инициализация словаря
        self.dictionaryOfGreetings <- {
            "Hello!": <- create Greeting(_language: "English"),
            "Bonjour!": <- create Greeting(_language: "French")
        }
    }
}
```

❗ Ссылки имеют префикс `&` и при её создании требуется приведение типа `as &Type`

## Интерфейсы

Интерфейсы - механизм взаимодействия с объектами. 

С их помощью можно унифицировать некоторые детали реализации так, чтобы с разными объектами можно было работать одинаково.

❗ Интерфейсы задают свод правил, которым должны удовлетворять объекты, реализующие этот интерфейс.

```swift
pub contract Stuff {
    // объявление интерфейса для ресурса
    pub resource interface ITest {
        pub let name: String
    }

    // объявление ресурса, реализующего интерфейс ITest
    pub resource Test: ITest {
        pub let name: String
        pub let number: Int
        init() {
            self.name = "Spongebob"
            self.number = 1
        }
    }

    // обращение к полю без интерфейса
    pub fun noInterface() {
        let test: @Test <- create Test()
        log(test.number) // 1
        destroy test
    }

    // обращение к полю через интерфейс
    pub fun yesInterface() {
        let test: @Test{ITest} <- create Test()
        log(test.number) // Ошибка, т.к. ITest ничего не знает о поле number ресурса Test
        destroy test
    }
}
```

Конструкция `Test{ITest}` называется ограниченным типом ([restricted type](https://docs.onflow.org/cadence/language/restricted-types/)). И у такого типа мы можем обращаться только к тем полям, что есть у ограничивающего интерфейса.

💡 Хорошей практикой считается для интерфейсов указывать префикс "I".

## Контроль доступа

 - `pub(set)`<br>
	Зона чтения: Везде<br>
	Зона записи: Везде
 - `pub` / `access(all)`<br>
	Зона чтения: Везде<br>
	Зона записи: Текущая и вложенные
 - `access(account)`<br>
	Зона чтения: Текущая, вложенные и все зоны контрактов, связанных с текущим аккаунтом<br>
	Зона записи: Текущая и вложенные
 - `access(contract)`<br>
	Зона чтения: Текущая, вложенные и текущий контракт<br>
	Зона записи: Текущая и вложенные
 - `priv` / `access(self)`<br>
	Зона чтения: Текущая и вложенные<br>
	Зона записи: Текущая и вложенные

[Подробнее](https://docs.onflow.org/cadence/language/access-control/)

## Пред/пост условия

Пред/пост условия позволяют делать проверки перед/после выполнения функции. Такие проверки несут дополнительный слой безопасности.

Также предусловия позволяют уменьшить комиссию. Так как за каждую операцию приходится платить, то чем раньше мы поймем, что двигаться дальше нет смысла, тем мы больше сэкономим комиссионных.

Пример с предусловием:

```swift
pub contract Test {

    pub fun logName(name: String) {
        // этот блок и есть предусловие
        pre {
            // условие должно быть истинным, иначе выполнение
            // прервется с сообщением об ошибке после двоеточия
            name.length > 0: "This name is too short."
        }

        // основной код функции
        log(name)
    }
}
```

Пример чуть посложнее с постусловием:

```swift
pub contract Test {
    pub var number: Int

    pub fun updateNumber(): Int {
        // блок постусловия
        post {
            // с помощью before мы можем получить значение
            // переменной до выполнения функции
            before(self.number) == self.number - 1
            // result - автоматическая переменная, которая
            // равна возвращаемому значению функции
            before(self.number) == result
        }
        
        let old_number = self.number;
        self.number = self.number + 1
        return old_number
    }

    init() {
        self.number = 0
    }
}
```

❗ Важно понимать, что прерывание транзакции отменяет все изменения состояния, что были сделаны функцией до ошибки.

## События

События - это способ смарт-контракта сообщить внешнему миру о том, что что-то произошло.

Например, если мы минтим NFT и хотим, чтобы внешний мир знал об этом, мы транслируем событие. И те, кто хотел получить оповещение, получат его. Это куда эффективней, чем бесконечно проверять, изменилось ли состояние.

```swift
pub contract Test {
    // объявляем событие
    pub event NFTMinted(id: UInt64)

    pub resource NFT {
        pub let id: UInt64

        init() {
            self.id = self.uuid

            // транслируем событие при создании
            emit NFTMinted(id: self.id)
        }
    }
}
```

## Скрипты

Позволяют читать данные из блокчейна. Не требуют оплаты комиссий.

```swift
pub fun main(): Type {
    ...
    return something // не обязательно
}
```

## Транзакции

Позволяют изменять данные. Требуют оплаты комиссий.

```swift
transaction(параметры) {
    prepare(signer: AuthAccount) { ... }
    execute { ... }
}
```

Транзакция имеет две секции:
 - `prepare` - стадия подготовки. Позволяет получить доступ к информации об аккаунте (с помощью параметра `signer: AuthAccount`) и сделать некоторые проверки.
 - `execute` - основная стадия. Используется для вызова функций и изменения данных в блокчейне.

💡 Технически, всё можно написать в секции `prepare`, но рекомендуется разделять логику.

## Аккаунты

Аккаунты в блокчейне Flow в отличии от Ethereum хранят свои данные. То есть, например, вместо того чтобы хранить адреса владельцев в контракте NFT, как это делается в Ethereum, сами аккаунты хранят информацию о том, что они владеют тем, или иным NFT.

Для доступа к информации аккаунта используют два типа: `PublicAccount` и `AuthAccount`. 

Забегая вперёд скажу, что аккаунты хранят:

 - Код контрактов, развернутых под этим аккаунтом (контрактов может быть несколько)
 - [Хранилище аккаунта](#хранилище-аккаунта)

Также Jacob сделал очень хорошую наглядную [картинку](https://github.com/jacob-tucker/Flow-Zero-to-Jacob/tree/main/chapter4.0/day1#what-lives-in-an-account).

## Хранилище аккаунта 

Хранилище делится на 3 раздела:

 - `/storage/` - доступен только для владельца аккаунта, и только он может изменять там данные. Именно тут живут данные, принадлежащие аккаунту.
 - `/public/` - доступен для всех.
 - `/private/` - доступен для владельца аккаунта и другим аккаунтам, кому владелец дал доступ.

❗ Важно понимать, что доступ к `/storage/` происходит через `AuthAccount`, который передается в секцию `prepare` при совершении транзакции, поэтому необходимо обращать внимание на то, что вы подписываете.

### Раздел `/storage/`

Функции:

 - `fun save<T>(_ value: T, to: StoragePath)` - сохранение значения `value` типа `T` по пути `StoragePath`.
 - `fun load<T>(from: StoragePath): T?` - извлечение данных типа `T` по пути `StoragePath`.
 - `fun borrow<T: &Any>(from: StoragePath): T?` - получение ссылки на данные типа `T` по пути `StoragePath`

Пара замечаний:

1. Все функции являются методами объектов типа `AuthAccount`.
2. Конструкция `<T>` между именем функции и списком параметров указывает тип данных, с которым работает функция. Это требуется поскольку мы не знаем, что за данные хранятся в `StoragePath`.
3. `StoragePath` представляет из себя просто путь `/storage/любой/путь`.
4. `load` и `borrow` возвращают опционал, так как мы не знаем наверняка, хранится ли что-то по переданному пути.

### Разделы `/public/` и `/private/`

На самом деле в этих разделах ничего не хранится, они являются некоторыми "интерфейсами" для взаимодействия с данными в `/storage/`. Для того, чтобы позволить обращаться к данным в `/storage/`, мы должны привязять к некоторому пути в `/public/` путь в `/storage/`, при этом создавая, как говорят, [capability](https://docs.onflow.org/cadence/language/capability-based-access-control/).

Вот опять замечательная [иллюстрация](https://github.com/jacob-tucker/Flow-Zero-to-Jacob/tree/main/chapter4.0/day2#capabilities) от Jacob.

❗ К `/public/` capabilities доступ имеет `PublicAccount`, а к `/private/` - `AuthAccount`

Связывание происходит с помощью функции `link`:<br>
`fun link<T: &Any>(_ newCapabilityPath: CapabilityPath, target: Path): Capability<T>?`

 - `newCapabilityPath` - это путь в `/public/` или `/private/`
 - `target` - это путь в `/storage/`

💡 При связывании мы можем ограничить наш ресурс публичным интерфейсом, используя механизм ограниченных типов.

## Контракты

```swift
[модификатор доступа] contract [имя контракта] {
    // переменные
    pub var someVar: Type

    // конструктор
    init() {
        self.someVar = ...
        ...
    }

    // функции
    pub fun someFunc(params) { ... }
}
```

 - Конструктор вызывается при развёртывании контракта единожды и выполняет инициализирующую логику.
 - Ключевое слово `self` используется для обращения к полям контракта внутри самого контракта.

# Практика

## Квесты

### Глава 2, День 1

1. Развернуть контракт с именем "JacobTucker" по адресу `0x03`:
    - контракт должен содержать константу `is` типа `String`;
    - проинициализировать константу строкой "the best".
2. Убедиться, что константа `is` действительно имеет значение "the best".

[Flow Playground](https://play.onflow.org/81e22fae-ed74-4c83-b03c-6c073e49f313)

### Глава 2, День 2

1. Создать контракт, содержащий:
	 - переменную `myNumber` типа `Int` (проинициализировать нулём);
	 - функцию `updateMyNumber`, которая принимает на вход параметр `newNumber` типа `Int` и присваивает значение к `myNumber`.
2. Написать скрипт для проверки значения `myNumber`.
3. Создать транзакцию с параметром `myNewNumber`, которая вызывает функцию `updateMyNumber`, и убедиться, что число действительно изменилось, запустив снова скрипт.

[Flow Playground](https://play.onflow.org/b60be3e3-10df-473d-90d6-f5b0df0a182f)

### Глава 2, День 3

1. В скрипте инициализировать массив длины 3, хранящий любимых людей, представленных типом `String`, и вывести их на экран.
2. В скрипте инициализировать словарь, где ключам соответствуют: *Facebook*, *Instagram*, *Twitter*, *YouTube*, *Reddit* и *LinkedIn* с типом `String`, а значениям соответствуют числа типа `UInt64` обозначающие частоту использования платформы (0 - если не использовалась).
3. Объяснить:
	 - Что означает ошибка?
	 - Почему мы получаем эту ошибку?
	 - Как исправить эту ошибку?

    ```swift
    pub fun main(): String {
        let thing: {Address: String} = { 0x01: "One", 0x02: "Two", 0x03: "Three" }

        return thing[0x03] // <- Mismatched types. Expect String, got String?
    }
    ```

[Flow Playground](https://play.onflow.org/d5b840bf-2374-4779-82d8-c7ec9c86780f)

### Глава 2, День 4

1. Создать контракт и объявить в нём структуру на свой выбор.
2. Объявить массив или словарь структур.
3. Реализовать функцию добавления новой структуры в массив/словарь.
4. Написать транзакцию, которая вызывает функцию из пункта 3.
5. Создать скрипт для чтения массива/словаря структур.

[Flow Playground](https://play.onflow.org/21078363-d042-46df-b981-5112593d8846)

### Глава 3, День 1

Найти и исправить 4 ошибки:

```swift
pub contract Test {
    // в объявлении ресурса ошибок нет
    pub resource Jacob {
        pub let rocks: Bool
        init() {
            self.rocks = true
        }
    }

    pub fun createJacob(): Jacob { // здесь одна ошибка
        let myJacob = Jacob() // две здесь
        return myJacob // и ещё одна здесь
    }
}
```

[Flow Playground](https://play.onflow.org/77d2e459-2bcc-4476-bf88-9a88e2b49bf1)

### Глава 3, День 2-3

1. Создать контракт, содержащий массив ресурсов и словарь ресурсов.
2. Для массива и словаря создать по паре функций: добавления и удаления ресурса.
3. Написать скрипт для чтения массива или словаря ресурсов.

[Flow Playground](https://play.onflow.org/1128961f-c703-4586-9fe4-8c5c2ff1549c)

### Глава 3, День 4

1. Создать контракт содержащий:

	 - интерфейс ресурса;
	 - ресурс, реализующий интерфейс;
	 - две функции.

	Первая функция должна обращаться к полям ресурса без интерфейса, а вторая - через интерфейс. Показать, к каким полям стало невозможно обратиться во втором случае.

2. Исправить ошибки

    ```swift
    pub contract Stuff {
        pub struct interface ITest {
            pub var greeting: String
            pub var favouriteFruit: String
        }

        // ERROR:`structure Stuff.Test does not 
        // conform to structure interface Stuff.ITest`
        pub struct Test: ITest {
            pub var greeting: String

            pub fun changeGreeting(newGreeting: String): String {
                self.greeting = newGreeting
                return self.greeting // returns the new greeting
            }

            init() {
                self.greeting = "Hello!"
            }
        }

        pub fun fixThis() {
            let test: Test{ITest} = Test()

            // ERROR HERE: `member of restricted type is not accessible: changeGreeting`
            let newGreeting = test.changeGreeting(newGreeting: "Bonjour!")

            log(newGreeting)
        }
    }
    ```

[Flow Playground](https://play.onflow.org/6ff14320-0de1-4d19-8861-78e170448848)

### Глава 3, День 5

Для каждой из четырёх зон написать:
1. какие из переменные из `a`, `b`, `c` и `d` могут быть там прочитаны;
2. какие из переменные из `a`, `b`, `c` и `d` могут быть там изменены;
3. какие из функций `publicFunc`, `contractFunc` и `privateFunc` могут быть там вызваны.

```swift
access(all) contract SomeContract {
    pub var testStruct: SomeStruct

    pub struct SomeStruct {
        // 4 Variables
        pub(set)         var a: String
        pub              var b: String
        access(contract) var c: String
        access(self)     var d: String

        // 3 Functions
        pub              fun publicFunc() {}
        access(contract) fun contractFunc() {}
        access(self)     fun privateFunc() {}

        pub fun structFunc() {
            /**************/
            /*** AREA 1 ***/
            /**************/
        }

        init() {
            self.a = "a"
            self.b = "b"
            self.c = "c"
            self.d = "d"
        }
    }

    pub resource SomeResource {
        pub var e: Int

        pub fun resourceFunc() {
            /**************/
            /*** AREA 2 ***/
            /**************/
        }

        init() {
            self.e = 17
        }
    }

    pub fun createSomeResource(): @SomeResource {
        return <- create SomeResource()
    }

    pub fun questsAreFun() {
        /**************/
        /*** AREA 3 ****/
        /**************/
    }

    init() {
        self.testStruct = SomeStruct()
    }
}
```

```swift
import SomeContract from 0x01

pub fun main() {
    /**************/
    /*** AREA 4 ***/
    /**************/
}
```

[Flow Playground](https://play.onflow.org/3fc21783-1472-4174-bde7-ab778896fb4e)

### Глава 4, День 1

Создать контракт, содержащий:

 - Ресурс хотя бы с одним полем;
 - Функцию, которая создаёт ресурс и возвращает его.

И написать две транзакции:

  - Первая транзакция должна сохранять ресурс в хранилище, сразу же его извлекать, затем печатать поле ресурса и уничтожать его.
  - Вторая транзакция должна также сохранять ресурс, но брать уже ссылку на ресурс и печатать поле.

[Flow Playground](https://play.onflow.org/d5e88ce9-e084-428e-af65-1a1a3753ca35)

### Глава 4, День 2

1. Создать контракт, содержащий ресурс, который реализует некоторый интерфейс.
2. Написать транзакцию, которая сохраняет ресурс и связывает его с публичным ограничивающим интерфейсом.
3. Попытаться прочитать с помощью скрипта скрытое поле.
4. Прочитать публичное поле и вернуть его из скрипта.

[Flow Playground](https://play.onflow.org/e84b2db7-71fa-4127-bf99-eeda49212035)

### Глава 5, День 1

1. Развернуть контракт, создать событие на свой выбор и транслировать его, указав, что произошло. Добавить пред/пост условия.
2. Для каждой из приведенных ниже функций ответить на вопросы.

    ```swift
    pub contract Test {

        // Напечатает ли эта функция `name`?
        // name: 'Jacob'
        pub fun numberOne(name: String) {
            pre {
                name.length == 5: "This name is not cool enough."
            }

            log(name)
        }

        // Вернёт ли значение эта функция?
        // name: 'Jacob'
        pub fun numberTwo(name: String): String {
            pre {
                name.length >= 0: "You must input a valid name."
            }
            post {
                result == "Jacob Tucker"
            }

            return name.concat(" Tucker")
        }

        pub resource TestResource {
            pub var number: Int

            // Изменится ли значение `number` после выполнения функции?
            // И какое значение будет?
            pub fun numberThree(): Int {
                post {
                    before(self.number) == result + 1
                }

                self.number = self.number + 1
                return self.number
            }

            init() {
                self.number = 0
            }
        }
    }
    ```

[Flow Playground](https://play.onflow.org/54c1bc56-7cf9-4001-8705-28680f3ddfde)

## Создаём собственный NFT смарт-контракт

### Глава 4, День 3

Немного о деталях реализации:
 - Контракт будет отображать некоторую коллекцию NFT (в нашем случае "CryptoPoops")
 - Токен NFT будет является ресурсом, что логично, потому мы не хотим, чтобы его случайно можно было скопировать или потерять.

```swift
pub contract CryptoPoops {
    // общее кол-во NFT
    pub var totalSupply: UInt64

    // ресурс, представляющий NFT
    pub resource NFT {
        // уникальный id
        pub let id: UInt64

        init() {
            // каждый ресурс имеет собственный уникальный uuid
            self.id = self.uuid
        }
    }

    // функция создания NFT
    pub fun createNFT(): @NFT {
        return <- create NFT()
    }

    // инициализация
    init() {
        self.totalSupply = 0
    }
}
```

Хорошо, давайте теперь сохраним себе NFT и сделаем его публичным для чтения.

```swift
import CryptoPoops from 0x01

transaction() {
    prepare(signer: AuthAccount) {
        // сохраняем
        signer.save(<- CryptoPoops.createNFT(), to: /storage/MyNFT)

        // делаем общедоступным
        signer.link<&CryptoPoops.NFT>(/public/MyNFT, target: /storage/MyNFT)
    }
}
```

Попробуем теперь прочитать информацию из аккаунта.

```swift
import CryptoPoops from 0x01

pub fun main(address: Address): UInt64 {
    let nft = getAccount(address)               // получаем PublicAccount
        .getCapability(/public/MyNFT)           // получаем Capability для нашей NFT
        .borrow<&CryptoPoops.NFT>()             // берём ссылку на NFT
        ?? panic("An NFT does not exist here.") // паникуем, если NFT нет

    return nft.id                               // возвращаем id
}
```

Мы сделали NFT и научились сохранять его к себе, но в такой реализации есть один существенный минус: мы можем сохранить к себе только один NFT, так как по некоторому пути в `/storage/` может храниться только один ресурс.

Чтобы решить эту проблему, давайте создадим ресурс, который будет отвечать за хранение NFT.

```swift
pub contract CryptoPoops {
    ...

    pub resource Collection {
        // в этом словаре будут хранится наши NFT
        pub var ownedNFTs: @{UInt64: NFT}

        // добавление NFT в коллекцию
        pub fun deposit(token: @NFT) {
            self.ownedNFTs[token.id] <-! token
        }

        // извлечение NFT из коллекции по ID
        pub fun withdraw(withdrawID: UInt64): @NFT {
            let nft <- self.ownedNFTs.remove(key: withdrawID) 
                ?? panic("This NFT does not exist in this Collection.")
            return <- nft
        }

        // возвращает все ID тех NFT, что хранятся в коллекции
        pub fun getIDs(): [UInt64] {
            return self.ownedNFTs.keys
        }

        // инициализация
        init() {
            self.ownedNFTs <- {}
        }

        // уничтожение коллекции 
        destroy() {
            destroy self.ownedNFTs
        }
    }

    // создание пустого словаря
    pub fun createEmptyCollection(): @Collection {
        return <- create Collection()
    }

    ...
}
```

❗ Перед удалением коллекции нам необходимо удалить все содержащиеся в ней NFT. Иначе NFT будут утеряны, и мы не сможем с ними ничего сделать.

Конструкция `destroy() { destroy self.ownedNFTs }` сообщает, что при удалении коллекции будут удалены NFT. И это необходимо делать с любыми вложенными ресурсами.

Теперь создадим коллекцию и сделаем её доступной для публики.

```swift
import CryptoPoops from 0x01

transaction() {
    prepare(signer: AuthAccount) {
        signer.save(<- CryptoPoops.createEmptyCollection(), to: /storage/MyCollection)
        signer.link<&CryptoPoops.Collection>(/public/MyCollection, target: /storage/MyCollection)
    }
}
```

Теперь каждый может посмотреть, какие есть NFT в коллекций, сделать депозит или вывести. И вот последнее нас не очень радует (мы же не хотим, чтобы наши NFT мог вывести кто угодно?). Тут на помощь придут интерфейсы и ограниченные типы.

```swift
pub contract CryptoPoops {
    ...

    // общедоступный интерфейс
    pub resource interface CollectionPublic {
        pub fun deposit(token: @NFT)
        pub fun getIDs(): [UInt64]
    }

    pub resource Collection: CollectionPublic { ... }

    ...
}
```

Таким образом, никто, кроме нас, не сможет выводить NFT из коллекции.

```swift
import CryptoPoops from 0x01

transaction() {
    prepare(signer: AuthAccount) {
        signer.save(<- CryptoPoops.createEmptyCollection(), to: /storage/MyCollection)

        signer.link<&CryptoPoops.Collection{CryptoPoops.CollectionPublic}>(
            /public/MyCollection,
            target: /storage/MyCollection
        )
    }
}
```

Поэкспериментируем с нашим контрактом:

1. Попробуем добавить и вывести NFT из своей коллекции.

    ```swift
    import CryptoPoops from 0x01

    transaction() {
        prepare(signer: AuthAccount) {
            // получаем ссылку на свою коллекцию
            let collection = signer.borrow<&CryptoPoops.Collection>(from: /storage/MyCollection)

            // делаем депозит
            collection.deposit(token: <- CryptoPoops.createNFT())

            // проверяем, что NFT сохранена в коллекции
            log(collection.getIDs()) // [2353]

            // выводим NFT
            let nft <- collection.withdraw(withdrawID: 2353)

            // проверяем, что в коллекции ничего не осталось
            log(collection.getIDs()) // []

            // уничтожаем NFT
            destroy nft
        }
    }
    ```

2. Попробуем отправить кому-то NFT.

    ```swift
    import CryptoPoops from 0x01

    // транзакция имеет параметр: адрес получателя
    transaction(recipient: Address) {
        prepare(otherPerson: AuthAccount) {
            // получаем ссылку на публичный интерфейс чужой коллекции
            let recipientsCollection = getAccount(recipient)
                .getCapability(/public/MyCollection)
                .borrow<&CryptoPoops.Collection{CryptoPoops.CollectionPublic}>()
                ?? panic("The recipient does not have a Collection.")

            // делаем депозит
            recipientsCollection.deposit(token: <- CryptoPoops.createNFT())
        }
    }
    ```

3. Попробуем вывести у кого-то NFT.

    ```swift
    import CryptoPoops from 0x01

    transaction(recipient: Address, withdrawID: UInt64) {
        prepare(otherPerson: AuthAccount) {
            // получаем ссылку на публичный интерфейс чужой коллекции
            let recipientsCollection = getAccount(recipient)
                .getCapability(/public/MyCollection)
                .borrow<&CryptoPoops.Collection{CryptoPoops.CollectionPublic}>()
                ?? panic("The recipient does not have a Collection.")

            // Ошибка: "Member of restricted type is not accessible: withdraw"
            let token <- recipientsCollection.withdraw(withdrawID: withdrawID)

            destroy token
        }
    }
    ```

4. Прочитаем информацию о NFT.

    ```swift
    import CryptoPoops from 0x01

    pub fun main(address: Address): [UInt64] {
        let publicCollection = getAccount(address)
            .getCapability(/public/MyCollection)
            .borrow<&CryptoPoops.Collection{CryptoPoops.CollectionPublic}>()
            ?? panic("The address does not have a Collection.")

        return publicCollection.getIDs() // [2353]
    }
    ```

Вот мы и сделали наш NFT смарт-контракт. Как обычно весь код тут:<br>
[Flow Playground](https://play.onflow.org/4a49bf7b-2b3a-4f13-8f9f-9bcd5b7669ee)

В следующей части мы разработаем логику минтинга NFT, так как на текущий момент минтить токены может каждый, что не очень правильно, а также добавим возможность удобно читать информацию о NFT.

### Глава 4, День 4

Напишем более продвинутую транзакцию для нашего NFT смарт-контракта, а именно транзакцию перевода NFT от одного человека к другому.

```swift
import CryptoPoops from 0x01

// транзакция имеет два параметра: id - номер NFT отправителя, recipient - получатель NFT
transaction(id: UInt64, recipient: Address) {
    prepare(signer: AuthAccount) {
        // получаем ссылку на коллекцию отправителя
        let signersCollection = signer.borrow<&CryptoPoops.Collection>(from: /storage/MyCollection)
            ?? panic("Signer does not have a CryptoPoops Collection")

        // получаем ссылку на коллекцию получателя
        let recipientsCollection = getAccount(recipient)
            .getCapability(/public/MyCollection)
            .borrow<&CryptoPoops.Collection{CryptoPoops.CollectionPublic}>()
            ?? panic("The recipient does not have a CryptoPoops Collection.")

        // берём NFT из коллекции отправителя
        let nft <- signersCollection.withdraw(withdrawID: id)

        // делаем депозит в коллекцию получателя
        recipientsCollection.deposit(token: <- nft)
    }
}
```

Теперь займёмся механикой минтинга NFT. Напомню, что сейчас минтить может каждый, и чтобы решить эту проблему, предлагаю начать с ресурса, который бы минтил NFT.

```swift
pub contract CryptoPoops {
    ...

    // этот Minter будет позволять своим владельцам минтить NFT 
    pub resource Minter {
        // определение этой функции мы переместили из контракта внутрь ресурса
        pub fun createNFT(): @NFT {
            return <- create NFT()
        }
    }

    init() {
        self.totalSupply = 0

        // при развёртывании контракта Minter сохранится на аккаунт владельца
        self.account.save(<- create Minter(), to: /storage/Minter)
    }
}
```

Попробуем заминтить кому-нибудь NFT

```swift
import CryptoPoops from 0x01

transaction(recipient: Address) {
    prepare(signer: AuthAccount) {
        // получаем ссылку на Minter
        let minter = signer.borrow<&CryptoPoops.Minter>(from: /storage/Minter)
            ?? panic("This signer is not the one who deployed the contract.")

        // получаем ссылку на коллекцию получателя
        let recipientsCollection = getAccount(recipient)
            .getCapability(/public/MyCollection)
            .borrow<&CryptoPoops.Collection{CryptoPoops.CollectionPublic}>()
            ?? panic("The recipient does not have a Collection.")

        // создаём NFT с помощью Minter'а
        let nft <- minter.createNFT()

        // делаем депозит в коллекцию получателя
        recipientsCollection.deposit(token: <- nft)
    }
}
```

Добавим последнюю удобную функцию получения ссылки на NFT.

```swift
pub contract CryptoPoops {
    ...

    pub resource NFT {
        pub let id: UInt64

        // для примера мы добавили некоторые метаданные
        pub let name: String
        pub let favouriteFood: String
        pub let luckyNumber: Int

        ...
    }

    pub resource interface CollectionPublic {
        ...

        // делаем функцию получения ссылки публичной
        pub fun borrowNFT(id: UInt64): &NFT
    }

    pub resource Collection: CollectionPublic {
        pub var ownedNFTs: @{UInt64: NFT}

        ...

        // реализуем функцию
        pub fun borrowNFT(id: UInt64): &NFT {
            return &self.ownedNFTs[id] as &NFT
        }
    }

    ...
}
```

Теперь мы с лёгкостью можем читать метаданные в скриптах.

```swift
import CryptoPoops from 0x01

pub fun main(address: Address, id: id) {
    // как обычно получаем ссылку на коллекцию
    let publicCollection = getAccount(address)
        .getCapability(/public/MyCollection)
        .borrow<&CryptoPoops.Collection{CryptoPoops.CollectionPublic}>()
        ?? panic("The address does not have a Collection.")

    // с помощью новой функции получаем ссылку на NFT
    let nftRef: &CryptoPoops.NFT = publicCollection.borrowNFT(id: id)

    // читаем метаданные
    log(nftRef.name)
    log(nftRef.favouriteFood)
    log(nftRef.luckyNumber)
}
```

Весь сегодняшний код и даже больше здесь:<br>
[Flow Playground](https://play.onflow.org/66ee83c8-7b84-4537-8244-463c7fb66a3e)

# Литература

 - [Оригинальный курс](https://github.com/jacob-tucker/Flow-Zero-to-Jacob)
 - [Документация языка Cadence](https://docs.onflow.org/cadence/language/)