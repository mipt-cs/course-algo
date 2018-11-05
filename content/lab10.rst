Структуры данных
################

:date: 2018-11-05 22:00
:summary: Структуры данных
:status: published
 


.. default-role:: code

.. contents:: Содержание

.. role:: c(code)
   :language: cpp

Введение
========

При работе с большими проектами очень не удобно использовать исключительно стандартные для языка типы данных. Намного удобнее объединить несколько стандартных типов под одним именем и написать ряд функций для работы с ним. В C объектами такого рода являются структуры (в C++ присутствуют ещё классы)

Структуры
=========

Для описания структуры, в C/C++ используется служебное слово :c:`struct`. Например, для работы с информацией о студентах очень удобно использовать структуру данных следующего вида:

.. code-block:: cpp

  struct Student
  {
    char name[100];
    int age, group;
  };

Здесь: `Student` — **имя** структуры, `name`, `age` и `group` — **поля** структуры. Для доступа к полю необходимо использовать точку. Пример использования структуры `Student`:

.. code-block:: cpp

  #include <iostream>

  struct Student
  {
    char name[100]; // Student name
    int age, group; // Student age and group
  };

  void get_student(Student *S)
  {
    /**
     * read information about student from keyboard
     */
    std::cout << "Input student name : ";
    std::cin.getline(S->name, 100); // read name
    std::cout << "Input age and group: ";
    std::cin >> S->age >> (*S).group; // read age and group
  }

  void print_student(Student S)
  {
    /**
     * print student information to screen
     */
    std::cout << "Student: " << S.name << ". Age: " << S.age << ", group: " << S.group << std::endl;
  }

  int main()
  {

    Student stud1;
    get_student(&stud1); // read Student
    print_student(stud1);  // print Student
    stud1.age += 2;        // increase age on 2
    print_student(stud1);
    return 0;
  }

Упражнение №1
-------------

Скомпилируйте и проверьте работу программы.

**Обратите внимание:** внутри функции, где переменная :c:`S` — указатель на структуру, для обращения к полю необходимо её сначала разыменовать (:c:`(*S).group`) или же использовать оператор стрелка (:c:`S->age`)

**Важно:** *если Вам придётся использовать структуры в C — имейте в виду, что работа со структурами там отличается от C++*

Стэк
====

Стэком называется структура, сохраняющая в себе набор данных, при этом  при извлечении данных из набора достаются последние, сохранённые в набор.

Для работы со стеком удобно использовать следующие структуры:

.. code-block:: cpp

  struct node
  {
    node *next;
    int data;
  };

  struct my_stack
  {
    node *last;
  }

Здесь вы видите две структуры. :c:`my_stack` — стэк, хранящий информацию о верхнем элементе (:c:`node * last`). :c:`node` — элемент списка, хранящий информацию о следующем элементе списка (:c:`node *next`) и некоторые данных (:c:`int data`). Графически данную структуру можно представить следующим образом.

.. image:: {filename}/images/lab10/1.svg

Данные структуры необходимо снабдить набором функций, для работы с ними: создание, добавление и извлечение данных, печать и удаление.

.. code-block:: cpp
  
  // create new stack
  void create_stack(my_stack *S)
  {
    S->last = nullptr; // set last to nullptr to understand when it empty
  }
  
  // add new data to stack
  void push_to_stack(my_stack *S, int data)
  {
    node *new_node = new node;  // create new node
    new_node->data = data;    // save data to node
    new_node->next = S->last; // save address of previous data
    S->last = new_node;     // set new node as last added
  }
  
  // get last data from stack
  int pop_from_stack(my_stack *S)
  {
    assert(S->last != nullptr); // check stack is not empty
    node *old_node = S->last;   // save old node
    S->last = old_node->next;   // new last - next for current last
    int res = old_node->data; // save result data
    delete old_node;      // free unused memory
    return res;         // return result
  }
  
  // print all data from stack
  void print_stack(my_stack S)
  {
    node * current_node = S.last;   // set current_node is pointer to last
    while(current_node)         // while current_node is not pointer to nullptr
    {
      std::cout << current_node->data << " ";   // print current_node data
      current_node = current_node->next;      // set current_node to next
    }
    std::cout << std::endl;       // print end line symbol
  }
  
  // delete all data from stack
  void delete_stack(my_stack *S)
  {
    while(S->last){           // while stack in not empty
      pop_from_stack(S);        // remove last element
    }
  }

Все функции целесообразно поместить в отдельный файл.

Упражнение №2
-------------

Скачайте здесь__ проект. Посмотрите в файле `my_stack.cpp` реализацию стэка на односвязном списке, скомпилируйте (используйте предоставленный в работе `Makefile`) и запустите его. Программа запрашивает у пользователя последовательность чисел, заканчивающуюся `0`. Положительные числа добавляются в стек. Если встречается отрицательное число, то из стека извлекается число и печатается на экране.

.. __: {filename}/code/lab10/example.zip

Задача №1
---------

Допишите в `my_stack` функцию, которая возвращает текущую глубину стэка.


Задача №2
---------

Напишите, с использованием структуры `my_stack` программу, которая проверяет корректность скобок `()[]{}` во входной строке.

Дек
===

Дек (от англ. deque — double ended queue) — структура данных, представляющая из себя список элементов, в которой добавление новых элементов и удаление существующих производится с обоих концов.

Дек, содержащий два элемента выглядит следующим образом:

.. image:: {filename}/images/lab10/2.svg

Задача №3
---------

#. Создайте структуру :c:`my_deque`, соответствующую изображению выше.
#. Напишите следующие функции, для работы с :c:`my_deque`:

  #) :c:`void create_deque(my_deque *d);` — создание дека;
  #) :c:`bool empty(my_deque *d);` — является ли дек пустым;
  #) :c:`void push_back(my_deque *d, int data);` — добавление в конец;
  #) :c:`void push_front(my_deque *d, int data);` — добавление в начало;
  #) :c:`int pop_back(my_deque d);` — извлечение данных из конца
  #) :c:`int pop_front(my_deque d);` — извлечение из начала списка
  #) :c:`int get_data(my_deque d, int index);` — возвращает данные по индексу, оставляя их в деке. Первым данным соответствует индекс `0`, Если индекс отрицательный то нумерация идёт с конца списка.
  #) :c:`void push_index(my_deque d, int index, int data);` — добавление данных по индексу. Первым данным соответствует индекс `0`, Если индекс отрицательный то нумерация идёт с конца списка.
  #) :c:`int pop_index(my_deque d, int index);` — извлечение данных по индексу. Первым данным соответствует индекс `0`, Если индекс отрицательный то нумерация идёт с конца списка.
  #) :c:`int set_data(my_deque d, int index, int data);` — изменение данных по индексу. Первым данным соответствует индекс `0`, Если индекс отрицательный то нумерация идёт с конца списка.


Задача №4*
----------

Реализуйте методы для работы со структурой :c:`Long_math` (аналог `длинной арифметики`__).

.. __: https://ru.wikipedia.org/wiki/Длинная_арифметика

Структура :c:`Long_math`

.. code-block:: cpp

  struct Long_math
  {
    char * digits;
  }

Пример функции :c:`main()`, использующая :c:`Long_math`

.. code-block:: cpp

  int main()
  {
    Long_math a, b, c;
    int tmp_int

    std::cout << "Type first number ";
    std::cin >> tmp_int;                // let tmp_int = 7250
    a = create_long(tmp_int);           // a.digits = ['0', '5', '2', '7']
    std::cout << "Type second number ";
    std::cin >> tmp_int;                // let tmp_int = 2868
    b = create_long(tmp_int);           // b.digits = ['8', '6', '8', '2']
    c = long_add(a, b);                 // c.digits = ['8', '1', '1', '0', '1']

    print_long(a); std::cout << ' + ';  // print:
    print_long(b); std::cout << ' = ';  // 
    print_long(c); std::cout << "\n"    // 7250 + 2868 = 10118

    delete_long(a);                     //
    delete_long(b);                     // free memory
    delete_long(c);                     //

    return 0;
  }

Напишите дополнительно функции умножения :c:`Long_math` на 10 и на 2