# Дополнительное задание

Вы разрабатываете программу учета заказов в интернет-магазине. У вас есть набор данных о заказах, каждый из которых представлен в виде словаря с информацией о товарах, количестве и общей сумме заказа. Необходимо создать функции для обработки этих данных.

Список заказов представлен в виде словарей, пример:

```python
orders = [
    {'id': 1, 'products': {'item1': 2, 'item2': 1}, 'total': 150.0},
    {'id': 2, 'products': {'item2': 3, 'item3': 2}, 'total': 250.0},
]
```

1. Напишите функцию, которая принимает на вход список заказов и возвращает общую сумму.
2. Напишите функцию , которая принимает на вход список заказов и возвращает словарь с информацией о самом дорогом заказе.
3. Напишите функцию, которая принимает на вход список заказов и название продукта, а затем возвращает список заказов, включающих этот продукт.
