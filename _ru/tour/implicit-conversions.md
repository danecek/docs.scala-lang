---
layout: tour
title: Неявные Преобразования
partof: scala-tour
num: 27
language: ru
next-page: polymorphic-methods
previous-page: implicit-parameters
---

Неявные преобразования — это мощная функция Scala, применяемая в двух распространенных вариантах:

- разрешить пользователям предоставлять аргумент одного типа так, как если бы это был другой тип, чтобы избежать шаблонного.
- в Scala 2 для предоставления дополнительных членов запечатанным классам (заменены [методами расширения][exts] в Scala 3).

### Детальный разбор

{% tabs implicit-conversion-defn class=tabs-scala-version %}
{% tab 'Scala 2' %}

В Scala 2 неявное преобразование из типа `S` в тип `T` определяется либо [неявным классом]({% link _overviews/core/implicit-classes.md %}) `T`
с одним параметром типа `S`, [неявным значением]({% link _tour/implicit-parameters.md %}), которое имеет тип функции `S => T`,
либо неявным методом, преобразуемым в значение этого типа.

{% endtab %}
{% tab 'Scala 3' %}

В Scala 3 неявное преобразование из типа `S` в тип `T` определяется [экземпляром `given`]({% link _tour/implicit-parameters.md %}), который имеет тип `scala.Conversion[S, T]`.
Для совместимости со Scala 2 их также можно определить неявным методом (подробнее читайте во вкладке Scala 2).

{% endtab %}
{% endtabs %}

Неявные преобразования применяются в двух случаях:

1. Если выражение `e` имеет тип `S` и `S` не соответствует ожидаемому типу выражения `T`.
2. При выборе `e.m`, где `e` типа `S`, если селектор `m` не указывает на элемент `S`.

В первом случае ищется конверсия `c`, применимая к `e` и тип результата которой соответствует `T`.

Примером является передача `scala.Int`, например `x`, методу, который ожидает `scala.Long`.
В этом случае вставляется неявное преобразование `Int.int2long(x)`.

Во втором случае ищется преобразование `c`, применимое к `e` и результат которого содержит элемент с именем `m`.

Примером является сравнение двух строк `"foo" < "bar"`.
В этом случае `String` не имеет члена `<`, поэтому вставляется неявное преобразование `Predef.augmentString("foo") < "bar"`
(`scala.Predef` автоматически импортируется во все программы Scala).

Дополнительная литература: [Неявные преобразования (в книге Scala)]({% link _overviews/scala3-book/ca-implicit-conversions.md %}).

[exts]: {% link _overviews/scala3-book/ca-extension-methods.md %}
