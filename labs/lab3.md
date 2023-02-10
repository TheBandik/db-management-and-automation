# Лабораторная работа №3

## Задание №1

Необходимо создать базу данных. Она создаётся не в скрипте, а с помощью `HeidiSQL` или другого иного похожего инструмента. В базе данных должно быть не менее трёх, связанных между собой, таблиц. Таблицы можно создать как с помощью скрипта, так и с помощью инструмента.

## Задание №2

Для каждой таблицы необходимо реализовать инструменты для внесения и получения данных. Также нужно реализовать хотябы по одному инструменту удаления и обновления данных.

Все эти инструменты должны содержать в себе не только простые SQL-запросы, но и дополнительную логику, необходимую для полноценной работы программы с базой данных (преподаватель может помочь придумать эту логику).

## Задание №3

Скрипт должен быть зациклен и предоставлять возможность пользователю работать с базой данных через интерфейс (консоль, web, desktop и т.д.).

# Пример с лекции

```python
import pymysql


class DataBase():
    def connection(self):
        self.db = pymysql.connect(
            host='192.168.1.102',
            port=3306,
            user='student',
            password='Hn523cv-',
            db='shop',
        )

        self.cursor = self.db.cursor()
    
    def create_tables(self):

        self.connection()

        self.cursor.execute(
            'CREATE TABLE if not exists providers(' +
            'id int auto_increment primary key,' +
            'name varchar(255))'
        )

        self.cursor.execute(
            'create table if not exists products (' +
            'id int auto_increment primary key,' +
            'name varchar(255),' +
            'provider_id int,' +
            'FOREIGN KEY (provider_id) REFERENCES providers (id))'
        )
    
    def add_provider(self, provider):
        self.connection()

        self.cursor.execute(f"insert providers (name) values ('{provider}')")

        self.db.commit()
        self.db.close()
    
    def get_providers(self, id):
        self.connection()

        self.cursor.execute(f"select * from providers where id = {id}")

        data = self.cursor.fetchall()

        print(data)

        self.db.close()

    def add_product(self, name, provider):
        self.connection()

        self.cursor.execute(f"select id from providers where name = '{provider}'")
        provider_id = self.cursor.fetchone()[0]
        
        if provider_id:
            self.cursor.execute(f"select * from products where name = '{name}' and provider_id = {provider_id}")
            data = self.cursor.fetchone()
            if data is None:
                self.cursor.execute(f"insert products (name, provider_id) values ('{name}', {provider_id})")
                self.db.commit()
        
        self.db.close()


db = DataBase()
db.create_tables()
# db.add_provider('Шевченко')
# db.get_providers(2) 
db.add_product('Арбузы', 'Файзуллаев')
```