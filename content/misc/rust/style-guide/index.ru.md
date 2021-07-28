---
title: Оформление кода
---

Много полезного тут: https://rust-lang.github.io/rustfmt/

## Термины

- **struct** (объект) - набор функций и переменных выполняющих определённые задачи
- **trait** (интерфейс) - интерфейс для объекта. также известен как "типаж", "трэйт"
- **элемент** - функции, объекты, интерфейсы, константы, и тп.
- **lib** (библиотека) - набор элементов
- **generics** (дженерик) - обобщённый тип

## Отступы

- пробелы, каждый уровень - 4 пробела
- максимальная длина строки - 120 символов

## Пустые строки

- между блоками на первом уровне - две пустые строки
- в конце файла - одна пустая строка
- в конце функции перед возвратом значения - одна пустая строка, если в функции больше двух строк

## Импорт модулей

- импорт только в начале файла
- импорт делится на блоки:
    - стандартные модули - `std::xxx`
    - внешние модули - секция dependencies
    - модули проекта - `crate::xxx`
    - собственные модули - `self::xxx`
- между блоками одна пустая строка
- все блоки входят в один use - использовать вложенность для импорта элементов и модулей
- если из модуля импортируется несколько элементов каждый элемент должен быть в новой строке. self, если есть, всегда в первой строке.
- в конце каждой строки запятая, сортировка по алфавиту.
- элементы входящие в модуль с общим именем необходимо использовать вместе с именем модуля. Пример: `cmp::min`, `mem::replace`, `io::Result`, `io::Error`, и т.п.
- не использовать имя модуля, если имя элемент очевидно. Например все названия элементов модуля `net` очевидны и нет необходимости использовать в коде `net::TcpStream` или `time::Duration`
- при импорте интерфейса можно использовать `as _` - если интерфейс не используется явно

```
//example
use {
    std::{
        fmt,
        io::{
            self,
            Read as _,
        },
        net::{
            SocketAddr,
            TcpStream,
        },
    },

    crate::{
        client::HttpClient,
        request::Request,
    },
};
```

## Названия

### fn

- **fn snake_case** - имя переменных маленькими буквами, `_` для разделения слов, начинается с буквы

Возможные типы функций:

- `fn example` - приватная
- `pub fn example` - публичная
- `pub async fn example` - публичная асинхронная

### let

- **snake_case**
- если переменная не используется, в начало названия необходимо добавить `_`

### const, static

- **UPPERCASE** - большими буквами

### type, struct, trait

- **CamelCase** - большая буква в начале каждого слова составляющих название
- **UPPERCASE** - если название является акронимом до 3-х символов

## Блоки кода

- начало блока `{` в одной строке с названием, через пробел
    - исключение - функция с описанием generic-типов с помощью `where`, начало блока в новой строке. См. Дженерики
- конец блока `}` в новой строке

## Типизация

Rust язык с строгой типизацией, но позволяет не указывать тип явно. Тип определяется автоматически, если это возможно.

- не указывать тип если это уместно:
    - объявляется новая переменная с типом i32 - он используется по умолчанию
    - переменной присваивается значение определённого типа, например `let v = 1234u32` или переменной присваивается результат выполнения функции
    - тип переменной становится явным при инициализации: `let mut s = String::new()`
- для объявления типа используется двоеточие, пробел необходим только после двоеточия. Пример: `let v: u32;`

## Дженерик

Обобщённый тип данных. Позволяет:

- Описать интерфейс для объекта без указания определённого типа данных. Реализация интерфейса для конкретного объекта сама определит с каким типом данных будет работать
- Объявить дженерик с определённым интерфейсом, чтобы ограничить функциональные возможности типа (дженерик с ограничением)

Оформление:

- название дженерика указывается в угловых скобках, без пробелов сразу после имени функции. Пример: example-2
    - несколько названий перечисляются через запятую, пробел только после запятой
- для определения связанного интерфейса, после названия дженерика необходимо указать двоеточие и пробел. Пример: example-1
- для множественных интерфейсов необходимо использовать плюс разделённый пробелами. Пример: example-3
- если используется ограничение с обобщённым интерфейсом необходимо использовать ключевое слово where для улучшения читаемости кода. Пример: example-2
    - where указывается в новой строке, без отступа
    - описание интерфейсов указывается в новой строке, с отступом и запятой в конце
    - блок кода в новой строке без отступа

```
// example-1
fn new<R: AsRef<[u8]>>(bytes: R) -> String {
    ...
}

// example-2
fn new<S, F>(name: S, callback: F) -> String
where
    S: Into<String>,
    F: Fn(&str) -> bool,
{
    ...
}

// example-3
fn foo<D: Display + Debug>(value: D) {
    ...
}
```

## inline-функции

```
#[inline]
fn foo(value: u32) {
    ...
}
```

inline позволяет развернуть функцию вместо вызовы. Улучшает производительность, так как код выполняется линейно без вызова функции.

- Необходимо использовать в простых функциях:
    - функция запускает другую функцию
    - функция устанавливает или возвращает переменную
- Для увеличения производительности при вычислениях (шифрование)

## Документация

Документация бывает двух уровней:

- уровень модуля, описывается в начале файлов проекта или модуля, строка начинается с `//! `
- уровень элементов, описывается в самом начале элемента, строка начинается с `/// `

Обязательно 1 пробел перед началом текста. Пример `/// Function description`
