# Лабораторная работа №1
### Цели лабораторной работы💡:
1. Разработать библиотеку для работы со структурой данных на выбранном языке программирования (например, C/C++, Java, Golang).
2. Создать тестовую программу для демонстрации функциональности разработанной библиотеки.
3. Разработать систему тестов (Unit test, Google test), для проверки работоспособности и корректности библиотеки.
   
**Вариант №19** (Двумерный массив. Сортировка массива. Вставка элемента в
отсортированный массив. Поиск элемента в отсортированном массиве.
Объединение двух отсортированных массивов. Пересечение двух
отсортированных массивов.).

## Основные задачи: 
1. Выбрать язык программирования для реализации библиотеки в соответствии с индивидуальным заданием.
2. Разработать и реализовать библиотеку для работы с элементами, включая операции вставки и удаления элементов.
3. Написать тестовую программу, которая демонстрирует основные сценарии использования библиотеки.
4. Провести тестирование разработанной библиотеки, убедившись в ее правильной работе на различных входных данных.
## Список используемых понятий:
**Двумерный массив** – это массив, в котором для определения местоположения элемента в массиве нужно указать значения двух индексов


### Разбор кода:
1. **Функция ввода массива**

    Запрашивает у пользователя количество строк.

    Для каждой строки запрашивает элементы, разделенные пробелами.

    Возвращает двумерный массив (список списков).

```python
def input_arr():
    arr = []
    rows = int(input("Enter rows: "))

    for i in range(rows):
        row = list(map(int, input(f"Enter row {i+1}: ").split()))
        arr.append(row)
    print(arr)
    return arr

```
2. **Функция сортировки массива**

    Сортирует каждую строку массива с использованием алгоритма пузырьковой сортировки.

    Возвращает отсортированный массив.
   
```python
def srt(arr: list):

    for row in arr:
        n = len(row)
        for i in range(n):
            for j in range(0, n - i - 1):
                if row[j] > row[j+1]:
                    row[j], row[j+1] = row[j+1], row[j]
    return arr
```
3. **Функция поиска элемента в отсортированном массиве**
   
    Ищет элемент el в отсортированном массиве.

    Возвращает список позиций (индексов) элемента в формате (строка, столбец).

    Если элемент не найден, возвращает None.
   
```python
def search_sorted(sorted_arr, el: int):
    positions_of_el = []

    for i, row in enumerate(sorted_arr):
        for j in range(len(row)):
            if row[j] == el:
                tuple_position = (i, j)
                positions_of_el.append(tuple_position)

    if len(positions_of_el) > 0:
        return positions_of_el
    else:
        return None
```
4. **Функция объединения массивов**

    Объединяет два массива одинаковой длины, суммируя соответствующие элементы.

    Сортирует результирующий массив и возвращает его.

```python
def merge_array(arr1: list, arr2: list):
    length = len(arr1)
    merged_array = [0] * length

    for i in range(length):
        merged_array[i] = arr1[i] + arr2[i]

    merged_array.sort()
    return merged_array
```
5. **Функция нахождения уникальных строк в массиве**

Возвращает массив, содержащий только уникальные строки из исходного массива.

```python
def uniq_array(arr: list):
    uniq_arr = []

    for row in arr:
        if row not in uniq_arr:
            uniq_arr.append(row)
    return uniq_arr
```
6. **Функция нахождения пересечения массивов**

    Находит пересечение двух массивов (общие строки).

    Возвращает массив, содержащий только общие строки.

```python
def intersection_array(arr1: list, arr2: list):
    intersection_arr = []
    uniq_arr1 = uniq_array(arr1)
    uniq_arr2 = uniq_array(arr2)

    for row1 in uniq_arr1:
        for row2 in uniq_arr2:
            if row1 == row2:
                intersection_arr.append(row1)
    print(intersection_arr)
    return intersection_arr
```
## Реализация тестов 

Для проверки корректности работы функций написаны unit-тесты с использованием библиотеки pytest.

```python
import pytest
import random
from array import srt, search_sorted, merge_array, uniq_array, intersection_array

def generate_random_2d_array(rows, cols, min_val=0, max_val=100):
    return [[random.randint(min_val, max_val) for _ in range(cols)] for _ in range(rows)]

def test_srt():
    arr = generate_random_2d_array(3, 3)  # Генерация 3x3 массива
    sorted_arr = srt(arr)

    # Проверка, что каждая строка отсортирована
    for row in sorted_arr:
        assert row == sorted(row)

def test_search_sorted_found():
    sorted_arr = [[1, 2, 3], [4, 5, 6]]
    positions = search_sorted(sorted_arr, 5)
    assert positions == [(1, 1)]  # элемент 5 находится в позиции (1, 1)

def test_search_sorted_not_found():
    sorted_arr = [[1, 2, 3], [4, 5, 6]]
    positions = search_sorted(sorted_arr, 7)
    assert positions is None  # элемент 7 не найден

def test_merge_array():
    arr1 = generate_random_2d_array(2, 3)  # Генерация 2x3 массива
    arr2 = generate_random_2d_array(2, 3)  # Генерация 2x3 массива
    merged = merge_array(arr1, arr2)

    # Проверка, что результат имеет правильный размер
    assert len(merged) == 2  # Должно быть 2 строки
    assert len(merged[0]) == 6  # Каждая строка должна содержать 6 элементов
    assert len(merged[1]) == 6  # Каждая строка должна содержать 6 элементов

def test_uniq_array():
    arr = [[1, 2, 3], [1, 2, 3], [4, 5, 6]]
    unique = uniq_array(arr)
    assert unique == [[1, 2, 3], [4, 5, 6]]  # Проверка на уникальные строки

def test_intersection_array():
    arr1 = [[1, 2, 3], [4, 5, 6]]
    arr2 = [[4, 5, 6], [7, 8, 9]]
    intersection = intersection_array(arr1, arr2)
    assert intersection == [[4, 5, 6]]  # Проверка на пересечения

# Запуск тестов
if __name__ == "__main__":
    pytest.main()
```


## Запуск тестов

Для запуска тестов используйте следующую команду:

```bash
pytest
```


## Вывод:

В результате выполнения данной работы были получены следующие практические навыки:


-изучение структуры двумерный массив


-изучение базовых алгоритмов для работы со структурами типа двумерный массив


-изучение базовой работы с unit тестами








