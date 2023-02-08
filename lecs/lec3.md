# Работа с MySQL в Python с помощью библиотеки pymysql

## Установка библиотеки

```
pip install pymysql
```

## Подключение к базе данных

```python
db = pymysql.connect(
    host='host',
    port='port',
    user='user',
    password='password',
    db='db',
    charset='charset'
)

cursor = db.cursor()
```

### Работа с базой данных

Для отправки `SQL` запроса в базу данных используется:

```python
cursor.execute('select * from games')
```

Для получения результатов выполнения запроса используется:

```python
# Получение одной строки
data = cursor.fetchone()
# Получение всех строк
data = cursor.fetchall()
```

Для получения информации о столбацах запроса используется:

```python
metadata = cursor.description
```
После запросов, меняющих данные в базе, необходимо указывать сохранение с помощью:

```python
db.commit()
```

После окончания работы, курсор и подключение нужно закрыть:

```python
cursor.close()
db.close()
```