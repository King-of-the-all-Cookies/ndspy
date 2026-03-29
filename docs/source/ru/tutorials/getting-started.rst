..
    Copyright 2019 RoadrunnerWMC

    This file is part of ndspy.

    ndspy is free software: you can redistribute it and/or modify
    it under the terms of the GNU General Public License as published by
    the Free Software Foundation, either version 3 of the License, or
    (at your option) any later version.

    ndspy is distributed in the hope that it will be useful,
    but WITHOUT ANY WARRANTY; without even the implied warranty of
    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
    GNU General Public License for more details.

    You should have received a copy of the GNU General Public License
    along with ndspy.  If not, see <https://www.gnu.org/licenses/>.

Урок 0: начало работы
=====================

Этот «урок» просто помогает вам подготовиться к остальным руководствам.

Прежде всего: если у вас ещё не установлен Python 3.6 или новее и ndspy,
обратитесь к разделу :ref:`installation-ru`.

Написание тестового скрипта
---------------------------

Руководства не предполагают работу в интерактивной оболочке Python. Вместо
этого мы показываем, как писать короткие скрипты — это мой предпочитаемый
способ взаимодействия с ndspy. Поэтому стоит убедиться, что вы умеете это
делать.

Создайте новый Python-файл (например, ``test.py``), откройте его в любимом
редакторе и вставьте следующее:

.. code-block:: python
    :linenos:

    import ndspy

    print('Hello world!')

Затем попробуйте запустить скрипт той копией Python, в которую вы установили
ndspy. Ваш редактор может предоставить встроенный способ запуска, либо надо
выполнить команду из терминала / командной строки, например:

.. code-block:: text

    python3 test.py

    py -3 -m test.py

Если скрипт выводит ``Hello world!`` и завершается, всё работает.

.. tip::

    Если вы получите такую ошибку:

    .. code-block:: text

        Traceback (most recent call last):
          File "test.py", line 1, in <module>
            import ndspy
        ModuleNotFoundError: No module named 'ndspy'

    Это означает, что вы запускаете ту копию Python, в которой ndspy не установлен.
    Попробуйте вместо просто ``3`` указать две первые цифры версии, которую вы
    установили, например ``python3.6 test.py``.
