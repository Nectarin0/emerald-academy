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

## Force-Unwrap оператор

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

❗ Ресурсы объявляются/создаются/уничтожаются только внутри одного контракта.

## Ресурсы в массивах

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

## Ресурсы в словарях

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
	
  // добавление в словарь | вариант 2
  pub fun addGreeting(greeting: @Greeting) {
    let key = greeting.message
        
    let oldGreeting <- self.dictionaryOfGreetings[key] <- greeting
    destroy oldGreeting
  }
	 
  // извлечение из словаря
  pub fun removeGreeting(key: String): @Greeting {
    let greeting <- self.dictionaryOfGreetings.remove(key: key) ?? panic("Could not find the greeting!")
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

В первом варианте добавления используется оператор принудительного перемещения `<-!`. Работает он так: если до добавления в словаре не было пары соответствующей ключу `key`, то по этому ключу будет записано новое значение, иначе произойдет ошибка.
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

Интерфейсы - механизм взаимодействия с объектами. С их помощью можно унифицировать некоторые детали реализации так, чтобы с разными объектами можно было работать одинаково. 
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

Хорошей практикой считается для интерфейсов указывать префикс "I".

## Контроль доступа

 - `pub(set)`
	Зона чтения: Везде
	Зона записи: Везде
 - `pub` / `access(all)`
	Зона чтения: Везде
	Зона записи: Текущая и вложенные
 - `access(account)`
	Зона чтения: Текущая, вложенные и все зоны контрактов, связанных с текущим аккаунтом
	Зона записи: Текущая и вложенные
 - `access(contract)`
	Зона чтения: Текущая, вложенные и текущий контракт
	Зона записи: Текущая и вложенные
 - `priv` / `access(self)`
	Зона чтения: Текущая и вложенные
	Зона записи: Текущая и вложенные

[Подробнее](https://docs.onflow.org/cadence/language/access-control/)

## Аккаунты

Аккаунты в блокчейне Flow в отличии от Ethereum хранят свои данные. То есть, например, вместо того чтобы хранить адреса владельцев в контракте NFT, как это делается в Ethereum, сами аккаунты хранят информацию о том, что они владеют тем, или иным NFT.
Для доступа к информации аккаунта используют два типа: `PublicAccount` и `AuthAccount`. Подробнее читать [тут](https://docs.onflow.org/cadence/language/accounts/).

## Скрипты

Позволяют читать данные из блокчейна. Не требуют оплаты комиссий.

```swift
pub fun main(): ReturnType {
  ...
  return something // не обязательно
}
```

## Транзакции

Позволяют менять данные. Требуют оплаты комиссий.

```swift
transaction(параметры) {
  prepare(signer: AuthAccount) { ... }
  execute { ... }
}
```

Транзакция имеет две секции:
 - `prepare` - стадия подготовки. Позволяет получить доступ к информации об аккаунте (с помощью параметра `signer: AuthAccount`) и сделать некоторые проверки.
 - `execute` - основная стадия. Используется для вызова функций и изменения данных в блокчейне.

Технически, всё можно написать в секции `prepare`, но рекомендуется разделять логику.

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

## Глава 2, День 1
1. Развернуть контракт с именем "JacobTucker" по адресу `0x03`:
    - контракт должен содержать константу `is` типа `String`;
    - проинициализировать константу строкой "the best".
2. Убедиться, что константа `is` действительно имеет значение "the best".

[Flow Playground](https://play.onflow.org/81e22fae-ed74-4c83-b03c-6c073e49f313?type=script&id=920766ab-1890-4618-9f81-aab06717eafa&storage=none)

## Глава 2, День 2

1. Создать контракт, содержащий: 
	 - переменную `myNumber` типа `Int` (проинициализировать нулём);
	 - функцию `updateMyNumber`, которая принимает на вход параметр `newNumber` типа `Int` и присваивает значение к `myNumber`.
2. Написать скрипт для проверки значения `myNumber`.
3. Создать транзакцию с параметром `myNewNumber`, которая вызывает функцию `updateMyNumber`, и убедиться, что число действительно изменилось, запустив снова скрипт.

[Flow Playground](https://play.onflow.org/b60be3e3-10df-473d-90d6-f5b0df0a182f?type=tx&id=94e1736a-d30e-4b79-bc12-2f2fc52a1d9d&storage=none)

## Глава 2, День 3

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

[Flow Playground](https://play.onflow.org/d5b840bf-2374-4779-82d8-c7ec9c86780f?type=script&id=b4d91a5b-7317-4359-8a1c-f5e335ed685c&storage=none)

## Глава 2, День 4

1. Создать контракт и объявить в нём структуру на свой выбор.
2. Объявить массив или словарь структур.
3. Реализовать функцию добавления новой структуры в массив/словарь.
4. Написать транзакцию, которая вызывает функцию из пункта 3.
5. Создать скрипт для чтения массива/словаря структур.

[Flow Playground](https://play.onflow.org/21078363-d042-46df-b981-5112593d8846?type=account&id=23e25429-0751-4373-844a-13e84822e575&storage=none)

## Глава 3, День 1

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

[Flow Playground](https://play.onflow.org/77d2e459-2bcc-4476-bf88-9a88e2b49bf1?type=account&id=b63f751e-71da-4e37-98c7-06e2dc9d3fb4&storage=none)

## Глава 3, День 2-3

1. Создать контракт, содержащий массив ресурсов и словарь ресурсов.
2. Для массива и словаря создать по паре функций: добавления и удаления ресурса.
3. Написать скрипт для чтения массива или словаря ресурсов.

[Flow Playground](https://play.onflow.org/1128961f-c703-4586-9fe4-8c5c2ff1549c?type=account&id=272ced8f-6145-4152-ab97-63a9ec7b02b3&storage=none)

## Глава 3, День 4

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
    
    // ERROR:
    // `structure Stuff.Test does not conform
    // to structure interface Stuff.ITest`
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
      let newGreeting = test.changeGreeting(newGreeting: "Bonjour!") // ERROR HERE: `member of restricted type is not accessible: changeGreeting`
      log(newGreeting)
    }
  }
  ```

[Flow Playground](https://play.onflow.org/6ff14320-0de1-4d19-8861-78e170448848?type=account&id=034dfea5-e3bb-4e41-a071-c308110202b0&storage=none)

## Глава 3, День 5

Для каждой из четырёх зон написать: 
1. какие из переменные из `a`, `b`, `c` и `d` могут быть там прочитаны;
2. какие из переменные из `a`, `b`, `c` и `d` могут быть там изменены;
3. какие из функций `publicFunc`, `contractFunc` и `privateFunc` могут быть там вызваны.

```swift
access(all) contract SomeContract {
  pub var testStruct: SomeStruct

  pub struct SomeStruct {

    //
    // 4 Variables
    //
    pub(set) var a: String

    pub var b: String

    access(contract) var c: String

    access(self) var d: String

    //
    // 3 Functions
    //
    pub fun publicFunc() {}

    access(contract) fun contractFunc() {}

    access(self) fun privateFunc() {}

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

[Flow Playground](https://play.onflow.org/3fc21783-1472-4174-bde7-ab778896fb4e?type=script&id=b103c6d2-f0a4-41ec-ba62-203df278237c&storage=none)

# Ссылки

 - [Оригинальный курс](https://github.com/jacob-tucker/Flow-Zero-to-Jacob)
 - [Документация языка Cadence](https://docs.onflow.org/cadence/language/)






