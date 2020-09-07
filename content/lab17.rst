Работа с ошибками в Python
##################################

:date: 2020-02-09 09:00
:summary: Рассматриваются некоторые типы ошибок и работа с Traceback. Ссылка на контест 1.
:status: draft

.. default-role:: code
.. contents:: Содержание


Заголовок
================
Создайте файл `solution.py` со следующим кодом:

.. code-block:: python
    :linenos:

    for coord in vector:
        print(coord)

Наш код подразумевает печать содержимого переменной vector.

Запустим написанный скрипт, получим следующий вывод:

.. code-block:: console

    $ python3 solution.py
    Traceback (most recent call last):
      File "solution.py", line 1, in <module>
        for coord in vector:
    NameError: name 'vector' is not defined

Сообщение означает, что при исполнении кода возникла ошибка.
При этом Python сообщает нам кое-что ещё.
Разберём это сообщение детально.

Чтение Traceback 1
------------------
Исходное сообщение нужно мысленно разделить на две части.
Первая часть это traceback-сообщение::

    Traceback (most recent call last):
      File "solution.py", line 1, in <module>
        for coord in vector:

Вторая часть - сообщение о возникшей ошибке::

    NameError: name 'vector' is not defined

Разберём первую часть.
Traceback в грубом переводе означает "отследить назад".
Traceback показывает *последовательность/стэк вызовов*, которая, в конечном итоге, вызвала ошибку.

Первая строка::

    Traceback (most recent call last):

является заголовочной.
Она сообщает, что в последующих строках будет изложен стэк вызовов (он показан отступами).
Обратите внимание на сообщение в скобках, оно указывает на порядок вызовов.
В данном случае (он же случай по умолчанию) тот вызов, в котором произошла ошибка, будет в последовательности вызовов указан последним.

Вторая и третья строки::

    File "solution.py", line 1, in <module>
      for coord in vector:

показывают информацию о вызове (в нашем случае он один).
Во-первых, здесь есть информация о файле, в котором произошёл вызов ("solution.py"), затем указан номер строки, где этот вызов происходит ("line 1"), в конце стоит информация о том, откуда произошёл вызов ("<module>").
В нашем случае вызов происходит непосредственно из модуля, т.е. не из функции.
Наконец, вывод содержит не только номер строки, но и саму строку "for coord in vector:".

Заключительная строка сообщения::

    NameError: name 'vector' is not defined

содержит вид (тип) ошибки ("NameError"), и после двоеточия содержит подсказку.
В данном случае она означает, что имя "vector" не определено.

В самом деле, если взглянуть снова на код, то можно убедиться, что мы нигде не объявили переменную "vector".

Подведём итоги.
При попытке запуска мы получили следующий вывод

.. code-block:: console

    $ python3 solution.py
    Traceback (most recent call last):
      File "solution.py", line 1, in <module>
        for coord in vector:
    NameError: name 'vector' is not defined

Он говорит нам о возникновении ошибки.
Эта ошибка обнаружилась интерпретатором в первой строке файла "solution.py".
Сама ошибка является ошибкой имени и указывает на необъявленное имя - "vector".

Чтение Traceback 2
------------------
Оберните код из solution.py в функцию:

.. code-block:: python
    :linenos:

    def print_vector(vector):
        for coord in vector:
            print(coord)

    print_vector(5)

Запустим наш код

.. code-block:: console
    
    $ python3 solution.py
    Traceback (most recent call last):
      File "solution.py", line 5, in <module>
        print_vector(5)
      File "solution.py", line 2, in print_vector
        for coord in vector:
    TypeError: 'int' object is not iterable

На этот раз сообщение об ошибке сложнее, однако структура у него та же.

Часть со стеком вызовов увеличилась::

    Traceback (most recent call last):
      File "solution.py", line 5, in <module>
        print_vector(5)
      File "solution.py", line 2, in print_vector
        for coord in vector:

Поскольку "most recent call last", читать будем её сверху вниз.

Вызовов на этот раз два.
Первый вызов::

      File "solution.py", line 5, in <module>
        print_vector(5)

Произошел в пятой строке.
Судя по строчке кода, это вызов написанной нами функции print_vector(5) с аргументом 5.

Следом за ней второй вызов::

          File "solution.py", line 2, in print_vector
            for coord in vector:

Этот вызов происходит *внутри* функции print_vector, содержащейся в файле "solution.py".
Вызов находится в строке 2.

Сама же ошибка имеет вид::

    TypeError: 'int' object is not iterable

Как и в первом примере, сообщение об ошибке содержит её тип и подсказку.
В нашем случае произошла ошибка типа.
В подсказке же указано, что объект типа int не является итерируемым, т.е. таким объектом, который нельзя использовать в цикле for.

Итог:

.. code-block:: console
    
    $ python3 solution.py
    Traceback (most recent call last):
      File "solution.py", line 5, in <module>
        print_vector(5)
      File "solution.py", line 2, in print_vector
        for coord in vector:
    TypeError: 'int' object is not iterable

В нашем коде возникла ошибка.
Её вызвала последовательность вызовов.
Первый вызов произошел непосредственно из модуля - в строке 5 происходит вызов функции print_vector(5).
Внутри этой функции ошибка возникла в строчке 2, содержащей проход по циклу.
Сообщение об ошибке означает, что итерироваться по объекту типа int нельзя.
В нашем случае мы вызвали функцию print_vector от числа (от 5).

Некоторые ошибки с примерами кода
=================================

Ошибки в синтаксисе
-------------------

Наиболее частая ошибка, которая возникает в программах на Python -- **SyntaxError**: когда какое-то утверждение записано не по правилам языка, например:

.. code-block:: pycon
    
    $ python3
    >>> print "hello"
      File "<stdin>", line 1
        print "hello"
                    ^
    SyntaxError: Missing parentheses in call to 'print'. Did you mean print("hello")?

Тот же тип ошибки возникнет, если забыть поставить двоеточие в цикле:

.. code-block:: pycon

    $ python3
    >>> for i in range(5)
      File "<stdin>", line 1
        for i in range(5)
                    ^
    SyntaxError: invalid syntax

При неправильном использовании пробелов и табуляций в начале строки возникает **IndentationError**:

.. code-block:: pycon
    
    $ python3
    >>> for i in range(5):
        print(i)
      File "<stdin>", line 2
        print(i)
            ^
    IndentationError: expected an indented block

А теперь посмотрим, что будет, если в первой строке цикла воспользоваться пробелами, а во второй - табуляцией:

.. code-block:: pycon
    
    $ python3
    >>> for i in range(5):
            print(i) # здесь пробелы
            print(i**2) # здесь табуляция
        File "<stdin>", line 3
          print(i**2)
                    ^
    TabError: inconsistent use of tabs and spaces in indentation
    
    
**NameError** возникает при обращении к несуществующей переменной:

.. code-block:: pycon
    
    $ python3
    >>> words = "Hello"
    >>> word
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    NameError: name 'word' is not defined

Ошибки в логике
---------------

Напишем простую программу на деление с остатком и сохраним как sample.py:

.. code-block:: python
    
    n = input()
    m = input()
    print(n % m)
    

и запустим её:

.. code-block:: pycon
    
    $ python3 sample.py
    5  
    3
    Traceback (most recent call last):
      File "sample.py", line 3, in <module>
        print(n % m)
    TypeError: not all arguments converted during string formatting
    
Возникла ошибка **TypeError**, которая сообщает о неподходящем типе данных. Исправим программу:

.. code-block:: python
    
    n = int(input())
    m = int(input())
    print(n % m)
    
запустим на неподходящих данных:

.. code-block:: pycon
    
    $ python3 sample.py
    xyz
    Traceback (most recent call last):
      File "sample.py", line 1, in <module>
        n = int(input())
    ValueError: invalid literal for int() with base 10: 'xyz'
    

Возникнет **ValueError**.
Эту ошибку ещё можно воспринимать как использование значения вне области допустимых значений (ОДЗ).

Теперь запустим программу на числовых данных:

.. code-block:: pycon
    
    $ python3 sample.py
    1
    0    
    Traceback (most recent call last):
      File "sample.py", line 3, in <module>
        print(n % m)
    ZeroDivisionError: integer division or modulo by zero
    
При работе с массивами нередко возникает ошибка **IndexError**. Она возникает при выходе за пределы массива:

.. code-block:: pycon
    
    $ python3
    >>> L1 = [1, 2, 3]
    >>> L1[3]
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    IndexError: list index out of range
    
Что будет, если вызвать бесконечную рекурсию? Опишем её в программе endless.py

.. code-block:: python
    
    def noend():
        print("Hello!")
        noend()
    noend()
    

Через некоторое время после запуска возникнет **RecursionError**:

.. code-block:: pycon
    
    Traceback (most recent call last):
      File "endless.py", line 4, in <module>
        noend()
      File "endless.py", line 3, in noend
        noend()
      File "endless.py", line 3, in noend
        noend()
      File "endless.py", line 3, in noend
        noend()
      [Previous line repeated 993 more times]
      File "endless.py", line 2, in noend
        print("Hello!")
    RecursionError: maximum recursion depth exceeded while calling a Python object
    
    

Контест №1
==========
Участвовать_ в контесте.

.. _Участвовать: http://judge2.vdi.mipt.ru/cgi-bin/new-client?contest_id=94114
