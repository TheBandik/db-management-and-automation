# Настройки безопасности баз данных

# Рекомендации

## Безопасная установка MySQL

+ Установка в защищенном окружении
+ Установка пароля на root
+ Отключение удаленного подключения к root

## Отключить LOCAL INFILE в MySQL

В рамках усиления безопасности необходимо отключить local-infile, чтобы предотвратить доступ к базовой файловой системе из MySQL, используя следующую директиву в разделе [mysqld].

```
# Путь
/etc/mysql/mysql.conf.d/mysqld.cnf [Debian/Ubuntu]
```

```
local-infile=0
```

## Измените порт MYSQL по умолчанию

Переменная Port устанавливает номер порта MySQL, который будет использоваться для прослушивания соединений TCP/IP. Номер порта по умолчанию – 3306, но вы можете изменить его в разделе [mysqld], как показано.

```
Port=1313
```

## Включить ведение журнала MySQL

Журналы — это один из лучших способов понять, что происходит на сервере, в случае любых атак вы можете легко увидеть любые действия, связанные с вторжением, из файлов журналов. Вы можете включить ведение журнала MySQL, добавив следующую переменную в разделе [mysqld].

```
log=/var/log/mysql.log
```

## Удалить историю оболочки MySQL

Все команды, которые вы выполняете в оболочке MySQL, хранятся клиентом mysql в файле истории: ~/.mysql_history. Это может быть опасно, поскольку для любых создаваемых вами учетных записей все имена пользователей и пароли, введенные в оболочке, будут записаны в файл истории.

```
cat /dev/null > ~/.mysql_history
```

## Не вводить пароль в командной строке

Так вот не надо:

```
mysql -u root -ppassword_
```

Надо вот так:

```
mysql -u root -p
```

## Определите пользователей базы данных приложения

Для каждого приложения, работающего на сервере, предоставьте доступ только пользователю, который отвечает за базу данных для данного приложения. Например, если у вас есть сайт WordPress, создайте определенного пользователя для базы данных сайта WordPress следующим образом.

```sql
CREATE DATABASE wordpress_db;
REATE USER 'wordpress'@'localhost' IDENTIFIED BY 'wordpress@dmin%!2';
GRANT ALL PRIVILEGES ON wordpress_db.* TO 'wordpress'@'localhost';
FLUSH PRIVILEGES;
exit
```

и не забывайте всегда удалять учетные записи пользователей, которые больше не управляют какой-либо базой данных приложений на сервере.

## Регулярно меняйте пароли MySQL

```sql
USE mysql;
UPDATE user SET password=PASSWORD('YourPasswordHere') WHERE User='root' AND Host = 'localhost';
FLUSH PRIVILEGES;
```

## Общие рекомендации

+ Никогда не давайте никому (кроме учетной записи MySQL root) доступ к таблице user в базе mysql
+ Не храните пароли открытого текста в своей базе данных. Злоумышленник может взять полный список паролей и использовать их. Вместо этого используйте SHA2(), SHA1(), MD5() или некоторую другую одностороннюю хеширующую функцию и храните значение хеша.

## Привелегии

+ CREATE
+ DROP
+ GRANT OPTION
+ LOCK TABLES
+ REFERENCES
+ EVENT
+ ALTER
+ DELETE
+ INDEX
+ INSERT
+ SELECT
+ UPDATE
+ CREATE TEMPORARY TABLES
+ TRIGGER
+ CREATE VIEW
+ SHOW VIEW
+ ALTER ROUTINE
+ CREATE ROUTINE
+ EXECUTE
+ FILE
+ CREATE TABLESPACE
+ CREATE USER
+ CREATE ROLE
+ DROP ROLE
+ PROCESS
+ PROXY
+ RELOAD
+ REPLICATION CLIENT
+ REPLICATION SLAVE
+ SHOW DATABASES
+ SHUTDOWN
+ SUPER
+ ALL
+ USAGE
