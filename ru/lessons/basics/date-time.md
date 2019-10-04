---
version: 1.0.1
title: Дата и время
---

Работа с временем в Elixir.

{% include toc.html %}

## Время

В Elixir есть несколько модулей для работы со временем. Стоит обратить
внимание, что эта функциональность работает только с временной зоной UTC.

Начнём с получения текущего времени:

```elixir
iex> Time.utc_now
~T[19:39:31.056226]
```

Обратите внимание на строковую метку, через которую также можно создать
структуру `Time`:

```elixir
iex> ~T[19:39:31.056226]
~T[19:39:31.056226]
```

Вы можете узнать больше о строковых метках в [уроке о строковых
метках](../sigils).

Можно легко получить части этой структуры:

```elixir
iex> t = ~T[19:39:31.056226]
~T[19:39:31.056226]
iex> t.hour
19
iex> t.minute
39
iex> t.day
** (KeyError) key :day not found in: ~T[19:39:31.056226]
```

Но тут есть загвоздка: как вы могли заметить, в этой структуре есть только
время в пределах суток, поэтому нет числа/месяца/года.

## Дата

В отличие от `Time`, структура `Date` содержит только текущую дату,
без какой-либо информации о времени.

```elixir
iex> Date.utc_today
~D[2028-10-21]
```

Кроме этого, есть полезные функции для работы с датами:

```elixir
iex> {:ok, date} = Date.new(2020, 12,12)
{:ok, ~D[2020-12-12]}
iex> Date.day_of_week date
6
iex> Date.leap_year? date
true
```

`day_of_week/1` определяет день недели указанной даты. В данном
случае это суббота. `leap_year?/1` определяет, является ли год високосным.
Остальные функции вы можете найти в [документации к
`Date`](https://hexdocs.pm/elixir/Date.html).

## NaiveDateTime

В Elixir существует два вида структур, содержащих и время, и дату. Первая -
`NaiveDateTime`. Её недостаток - отсутствие поддержки временных зон:

```elixir
iex(15)> NaiveDateTime.utc_now
~N[2029-01-21 19:55:10.008965]
```

Но она содержит как время, так и дату, так что можете попробовать добавить
времени, к примеру:

```elixir
iex> NaiveDateTime.add(~N[2018-10-01 00:00:14], 30)
~N[2018-10-01 00:00:44]
```

## DateTime

Вторая, как вы уже могли догадаться из названия раздела, называется `DateTime`.
У неё нет ограничений, упомянутых выше: она хранит как время, так и дату, а также поддерживает
временные зоны. Но будьте осторожны с временными зонами. Как сказано в
официальной документации:

```
Заметьте, в этом модуле находятся только функции преобразования и другие функции, работающие только с UTC.
Это из-за того, что правильная реализация DateTime нуждается в базе данных временных зон, которая на данный момент не поставляется вместе с Elixir.
```

Также обратите внимание, что можно создать `DateTime` из `NaiveDateTime`,
только передав временную зону:

```
iex> DateTime.from_naive(~N[2016-05-24 13:26:08.003], "Etc/UTC")
{:ok, #DateTime<2016-05-24 13:26:08.003Z>}
```

Вот и всё! Если вы хотите попробовать другие дополнительные функции, 
загляните в документацию:
- [Time](https://hexdocs.pm/elixir/Time.html)
- [Date](https://hexdocs.pm/elixir/Date.html)
- [DateTime](https://hexdocs.pm/elixir/DateTime.html)

Также рекомендуем изучить [Timex](https://github.com/bitwalker/timex) и
[Calendar](https://github.com/lau/calendar) - мощные библиотеки для работы с
временем в Elixir.