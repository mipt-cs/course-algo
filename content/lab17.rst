Работа с ошибками в Python
##################################

:date: 2020-02-10 09:00
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

Контест №1
==========
Участвовать_ в контесте.

.. _Участвовать: http://judge2.vdi.mipt.ru/cgi-bin/new-client?contest_id=94114