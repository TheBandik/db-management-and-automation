# Структурные элементы баз данных. Подзапросы

## Структурные элементы баз данных

### Таблицы

Таблицы - это основные структурные элементы базы данных. Они организуют данные в виде сетки, где каждая строка представляет отдельный объект или запись, а каждый столбец представляет атрибут этой записи.

Пример:

| ID  | Имя       | Фамилия  | Возраст |
| --- | --------- | -------- | ------- |
| 1   | Анна      | Иванова  | 25      |
| 2   | Иван      | Петров   | 30      |
| 3   | Мария     | Сидорова | 22      |

### Столбцы

Столбцы определяют атрибуты данных, которые могут храниться в таблице. Они также определяют типы данных для каждого атрибута.

Пример: В таблице `Пользователи` столбец `Имя` имеет тип данных `строка`, столбец `Возраст` имеет тип данных `целое число`.

### Строки

Строки представляют собой отдельные записи в таблице, содержащие информацию о конкретных объектах или сущностях.

Пример: В таблице `Пользователи` каждая строка представляет информацию о конкретном пользователе. Например, строка с `ID 1` содержит информацию о пользователе с именем `Анна Иванова` и возрастом `25` лет.

### Ключи

Ключи используются для уникальной идентификации записей в таблице. В базах данных существует два основных типа ключей:

- Первичные ключи (Primary Keys): Это уникальные столбцы, которые гарантируют, что каждая запись в таблице имеет уникальное значение ключа. Например, столбец `ID` в таблице `Пользователи` может быть установлен как первичный ключ.
- Внешние ключи (Foreign Keys): Эти ключи связывают записи в одной таблице с записями в другой таблице. Они используются для создания отношений между таблицами.

### Индексы

Индексы используются для ускорения поиска данных в таблице. Они представляют собой структуры данных, которые создаются на одном или нескольких столбцах и позволяют быстро находить нужные записи.

Пример: Допустим, у нас есть большая таблица `Товары`, и мы хотим быстро найти все товары с определенным названием. Создание индекса на столбце `Название товара` ускорит поиск и позволит быстро извлекать нужные данные.

## Подзапросы

### Определение

Подзапросы, также известные как вложенные запросы или внутренние запросы, представляют собой SQL-запросы, которые включены в другие SQL-запросы. Они позволяют выполнять сложные операции, состоящие из нескольких этапов, где результат одного запроса используется как входные данные для другого. Подзапросы часто используются для фильтрации, агрегации и сравнения данных в базе данных.

### Виды запросов

- Скалярные подзапросы: Эти подзапросы возвращают одно значение, которое может быть использовано в основном запросе. Например, скалярный подзапрос может быть использован для получения среднего значения или максимального значения из столбца.
- Табличные подзапросы: Эти подзапросы возвращают набор строк и столбцов, который может быть использован в качестве временной таблицы для выполнения операций JOIN, фильтрации или других манипуляций с данными.

### Применение подзапросов

```
<Запрос> in (<запрос>)
```

Пример:

```
select id, name, sales_worldwide, developer_id from games where sales_worldwide > 50000000 and developer_id in (select id from companies where name = 'Valve')
```
