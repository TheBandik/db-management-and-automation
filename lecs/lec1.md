# Процедуры

## Определение

Хранимая процедура - это способ инкапсуляции повторяющихся действий. В хранимых процедурах можно объявлять переменные, управлять потоками данных, а также применять другие техники программирования.

## Плюсы

+ Разделение логики с другими приложениями. Хранимые процедуры инкапсулируют функциональность; это обеспечивает связность доступа к данным и управления ими между различными приложениями
+ Изоляция пользователей от таблиц базы данных. Это позволяет давать доступ к хранимым процедурам, но не к самим данным таблиц
+ Обеспечивает механизм защиты. В соответствии с предыдущим пунктом, если вы можете получить доступ к данным только через хранимые процедуры, никто другой не сможет стереть ваши данные через команду ```DELETE```.
+ Улучшение выполнения как следствие сокращения сетевого трафика. С помощью хранимых процедур множество запросов могут быть объединены

## Минусы

+ Повышение нагрузки на сервер баз данных в связи с тем, что большая часть работы выполняется на серверной части, а меньшая - на клиентской
+ Вы дублируете логику своего приложения в двух местах: серверный код и код для хранимых процедур, тем самым усложняя процесс манипулирования данными
+ Миграция с одной СУБД на другую может привести к проблемам

## Ограничитель

Ограничитель - это символ или строка символов, который используется для указания клиенту MySQL, что вы завершили написание выражения SQL. Целую вечность ограничителем был символ точки с запятой. Тем не менее, могут возникнуть проблемы, так как в хранимой процедуре может быть несколько выражений, каждое из которых должно заканчиваться точкой с запятой. Обычно строка "//" используется в качестве ограничителя.

```
DELIMITER //
```

## Создание процедуры

Названия хранимых процедур чувствительны к регистру. Вам также нельзя создавать несколько процедур с одинаковым названием. Внутри хранимой процедуры не может быть выражений, изменяющих саму базу данных.

```
CREATE PROCEDURE procedure_name ()
    BEGIN
        # Запросы
    END//
```

## Вызов процедуры

Чтобы вызвать хранимую процедуру, необходимо напечатать ключевое слово CALL, а затем название процедуры, а в скобках указать параметры.

```
CALL procedure_name (param1, param2, ....)  
```

## Изменение хранимой процедуры

В MySQL есть выражение ALTER PROCEDURE для изменения процедур, но оно подходит для изменения лишь некоторых характеристик. Если вам нужно изменить параметры или тело процедуры, вам следует удалить и создать ее заново.

## Удаление хранимой процедуры

```
DROP PROCEDURE IF EXISTS procedure_name;
```

## Параметры

Как можно передавать в хранимую процедуру параметры:

+  Пустой список параметров 
  
```
CREATE PROCEDURE proc1 ()
```

+ Один входящий параметр. Слово IN необязательно, потому что параметры по умолчанию - IN (входящие)
  
```
CREATE PROCEDURE proc1 (IN varname DATA-TYPE)
```

+ Один возвращаемый параметр
  
```
CREATE PROCEDURE proc1 (OUT varname DATA-TYPE)
```

+ Один параметр, одновременно входящий и возвращаемый

```
CREATE PROCEDURE proc1 (INOUT varname DATA-TYPE)
```

Естественно, вы можете задавать несколько параметров разных типов.

### Пример IN

```
CREATE PROCEDURE proc_IN (IN var1 INT) 

BEGIN 
        SELECT var1 + 2 AS result; 
END//  
```

### Пример OUT

```
CREATE PROCEDURE OUT (OUT var1 VARCHAR(100) 

BEGIN 
        SET var1 = 3;
END//  
```

### Пример INOUT

```
CREATE PROCEDURE proc_INOUT (OUT var1 INT) 

BEGIN 
        SET var1 = var1 * 2;  
END//  
```

## Переменные

Вы должны объявлять их явно в начале блока BEGIN/END, вместе с их типами данных.

### Примеры объявлений

```
DECLARE a, b INT DEFAULT 5; 
DECLARE str VARCHAR(50); 
DECLARE today TIMESTAMP DEFAULT CURRENT_DATE; 
DECLARE v1, v2, v3 TINYINT;
```

## Пример использования переменных

```
CREATE PROCEDURE `var_proc` (IN paramstr VARCHAR(20)) 

BEGIN 

    DECLARE a, b INT DEFAULT 5; 

    DECLARE str VARCHAR(50); 

    DECLARE today TIMESTAMP DEFAULT CURRENT_DATE; 

    DECLARE v1, v2, v3 TINYINT;     

   
    INSERT INTO table1 VALUES (a); 

    SET str = 'Строка'; 

    SELECT CONCAT(str, paramstr), today FROM table2 WHERE b >= 5; 

END //  
```

## Структуры управления потоками

MySQL поддерживает конструкции IF, CASE, ITERATE, LEAVE LOOP, WHILE и REPEAT для управления потоками в пределах хранимой процедуры.

### Конструкция IF

С помощью конструкции IF, мы можем выполнять задачи, содержащие условия

#### Пример

```
CREATE PROCEDURE `proc_IF` (IN param1 INT) 

BEGIN 

    DECLARE variable1 INT; 

    SET variable1 = param1 + 1; 


    IF variable1 = 0 THEN 

        SELECT variable1; 

    END IF; 


    IF param1 = 0 THEN 

        SELECT 'Parameter value = 0'; 

    ELSE 

        SELECT 'Parameter value <> 0'; 

    END IF; 

END //  
```

### Конструкция CASE

CASE - это еще один метод проверки условий и выбора подходящего решения. Это отличный способ замены множества конструкций IF. Конструкцию можно описать двумя способами, предоставляя гибкость в управлении множеством условных выражений.

#### Примеры
```
CREATE PROCEDURE `proc_CASE` (IN param1 INT) 

BEGIN 

    DECLARE variable1 INT; 

    SET variable1 = param1 + 1; 
   

    CASE variable1 

        WHEN 0 THEN 

            INSERT INTO table1 VALUES (param1); 

        WHEN 1 THEN 

            INSERT INTO table1 VALUES (variable1); 

        ELSE 

            INSERT INTO table1 VALUES (99); 

    END CASE; 
   

END //  
```

или

```
CREATE PROCEDURE `proc_CASE` (IN param1 INT) 
BEGIN 

    DECLARE variable1 INT; 

    SET variable1 = param1 + 1; 
   

    CASE 

        WHEN variable1 = 0 THEN 

            INSERT INTO table1 VALUES (param1); 

        WHEN variable1 = 1 THEN 

            INSERT INTO table1 VALUES (variable1); 

        ELSE 

            INSERT INTO table1 VALUES (99); 

    END CASE; 
   

END // 
```

### Конструкция WHILE
Повторение кода в виде цикла.

#### Пример

```
CREATE PROCEDURE `proc_WHILE` (IN param1 INT) 

BEGIN 

    DECLARE variable1, variable2 INT; 

    SET variable1 = 0; 
   

    WHILE variable1 < param1 DO 

        INSERT INTO table1 VALUES (param1); 

        SELECT COUNT(*) INTO variable2 FROM table1; 

        SET variable1 = variable1 + 1; 

    END WHILE; 

END //  
```
