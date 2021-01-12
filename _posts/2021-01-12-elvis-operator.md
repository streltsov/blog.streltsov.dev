---
title: "Элвис оператор ?. в JavaScript"
---

Элвис оператор или **оператор опциональной последовательности** это очень крутая фича, добавленная в ES2020, которая позволяет писать более чистый, короткий и понятный код.

## Зачем нужен Элвис оператор?

Допустим у нас есть объект `book` который содержит в себе информацию о книге:

```js
const book = {
  title: "You Don't Know JS Yet",
  author: {
    firstName: "Kyle",
    lastName: "Simpson"
  }
};
```

При помощи точечной нотации мы можем получить фамилию автора:

```js
book.author.lastName // "Simpson"
```

Шикарно. Но что если API с которого мы получаем книжки, не вернет нам свойство `author`?


Тогда мы получим ошибку:

```js
book.author.lastName // TypeError: can't access property "lastName", book.author is undefined
```

С таким ненадежным API, который не гарантирует доставку всех нужных нам свойств, просто использовать конструкцию `book.author.lastName` становится небезопасно.

В случае когда нам нужно получить свойство, которое находится глубоко в структуре объекта, необходимо проверять существует ли свойство от которого мы пытаемся получить значение.

И обычно это делали с помощью оператора `&&`:

```js
const lastName = book.author && book.author.lastName
```

Теперь, если в объекте `book` нет свойства `author`, то по левую сторону от оператора `&&`, `book.author` вернет `undefined`, а значит правая сторона не будет вычисляться и все выражение `book.author && book.author.lastName` будет равно `undefined`.

Если `author` существует, то выполнится правая сторона и нам вернется фамилия автора.

С оператором опциональной последовательности эту проверку можно провести с меньшим количеством кода:

```js
const lastName = book?.author.lastName
```

35 символов против 21! Но, более впечатляющие результаты будут если объект будет иметь более глубокую структуру:

```js
const user = {
  address: {
    street: {
      name: "Elm Street"
    }
  }
}
```

С оператором `&&` нужно будет сделать несколько последовательных проверок:

```js
user.address && user.address.street && user.address.street.name
```

В то время как оператор опциональной последовательности делает это за нас:

```js
user.address?.street?.name
```
Уже 63 символа против 26! Думаю стало понятно чем полезен Элвис оператор и теперь перейдем к тому как его еще можно применять и как он работает.

## Опциональный вызов функции или метода

Элвис оператор можно использовать при вызове функции:

```js
fn?.(x)
```

Если `fn` это функция, то она вызовется с аргументом x. Если `fn` равно `undefined` или `null`, то выражение порсто вернет `undefined`. 


Why does foo?.() throw when foo is neither nullish nor callable?

    Imagine a library which will call a handler function, e.g. onChange, just when the user has provided it. If the user provides the number 3 instead of a function, the library will likely want to throw and inform the user of their mistaken usage. This is exactly what the proposed semantics for onChange?.() achieve.

    Moreover, this ensures that ?. has a consistent meaning in all cases. Instead of making calls a special case where we check typeof foo === 'function', we simply check foo == null across the board.

    Finally, remember that optional chaining is not an error-suppression mechanism.


## Как работает Элвис оператор

Если левая сторона оператора `?.` равна `undefined` или `null`, все выражение примет значение `undefined`:

```js
const foo = undefined;
foo?.bar // undefined
```

По сути, выражение `foo?.bar` эквивалентно следующему коду:

```js
if (foo === undefined || foo === null) {
  return undefined;
} else {
  return foo.bar;
}
```

## Сокращенное выполнение (short-circuiting)

## Производительность
Вероятно, вы заметили, что использование опциональных последовательностей означает дополнительную нагрузку на систему. Всё дело в том, что каждый раз, когда используется оператор ?., система вынуждена проводить дополнительные проверки. Злоупотребление оператором ?. может заметно повлиять на производительность программ.

Я посоветовал бы вам использовать эту возможность вместе с некоей системой проверки, которая позволяет анализировать объекты при их получении откуда-либо или при их создании. Это сократит необходимость в использовании конструкции ?. и ограничит её воздействие на производительность.
