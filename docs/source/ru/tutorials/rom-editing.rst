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



Tutorial 1: редактирование файла в ROM

=====================================



.. py:currentmodule:: ndspy.rom
    :noindex:




В этом уроке объясняется, как открыть ROM-файл, извлечь из него нужный

файл, вставить обратно отредактированную версию и сохранить изменённый ROM.



.. note::



    Мы проделаем это так, как делаю лично я: пишем небольшие временные скрипты,

    которые решают текущую задачу. Чтобы было проще следить, я каждый раз буду

    выкладывать весь скрипт, а не только исправленные строчки (хотя для удобства

    изменённые строки будут выделяться).





Открытие ROM

------------



Первый шаг — импортировать модуль ndspy, посвящённый ROM-файлам,

:py:mod:`ndspy.rom`. Создайте новый пустой Python-файл (например,

``rom_files_tutorial.py``), откройте его и вставьте следующее:



.. code-block:: python

    :linenos:



    import ndspy.rom



Попробуйте на этом этапе запустить файл. Если ошибок нет, всё в порядке.



Разумеется, нужен сам ROM. Для удобства положите его рядом со скриптом.

Я буду использовать *New Super Mario Bros.* (``nsmb.nds``), но то же самое

подойдёт почти для любого ROM.



:py:mod:`ndspy.rom` предоставляет класс :py:class:`NintendoDSRom`, с помощью

кого мы работаем с ROM. Его конструктор принимает :py:class:`bytes` с данными

файла, так что сначала надо получить такие байты. Шаг сам по себе не связан с

ndspy:



.. code-block:: python

    :emphasize-lines: 3-4

    :linenos:



    import ndspy.rom



    with open('nsmb.nds', 'rb') as f:

        data = f.read()



При вызове ``open`` аргумент ``'rb'`` говорит, что файл открывается для

чтения в бинарном режиме. Можно напечатать начало содержимого:



.. code-block:: python

    :emphasize-lines: 6

    :linenos:



    import ndspy.rom



    with open('nsmb.nds', 'rb') as f:

        data = f.read()



    print(data[:50])



.. code-block:: text



    b'NEW MARIO\x00\x00\x00A2DE01\x00\x00\x08\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00@\x00\x00\x00\x08\x00\x02\x00\x00\x00\x02\xa4\xef\x05\x00\x00\xe8'



Теперь можно передать эту переменную ndspy и получить объект

:py:class:`NintendoDSRom`, с которым можно дальше работать:



.. code-block:: python

    :emphasize-lines: 6-8

    :linenos:



    import ndspy.rom



    with open('nsmb.nds', 'rb') as f:

        data = f.read()



    rom = ndspy.rom.NintendoDSRom(data)



    print(rom)



.. code-block:: text



    <rom "NEW MARIO" (A2DE)>



Отлично, теперь у нас есть :py:class:`NintendoDSRom` для NSMB. (``NEW MARIO`` —

внутреннее имя игры. Оно может помочь, но не всегда совпадает с заголовком.)



.. warning::



    Я использую имя переменной ``rom`` для объекта :py:class:`NintendoDSRom`,

    потому что он представляет ROM. Не перепутайте это с :py:mod:`ndspy.rom`,

    модулем, в котором определён класс!



Поскольку открытие файла, чтение его содержимого и создание

:py:class:`NintendoDSRom` — частая операция, ndspy предоставляет сокращённый

способ:



.. code-block:: python

    :emphasize-lines: 3

    :linenos:



    import ndspy.rom



    rom = ndspy.rom.NintendoDSRom.fromFile('nsmb.nds')



    print(rom)



.. code-block:: text



    <rom "NEW MARIO" (A2DE)>



Как видите, это делает ровно то же самое. Во многих классах ndspy есть

аналогичные методы ``.fromFile(filename)``.



Теперь, когда у нас есть объект ROM, что с ним делать? Много чего! Например,

можно узнать количество вложенных файлов:



.. code-block:: python

    :emphasize-lines: 5

    :linenos:



    import ndspy.rom



    rom = ndspy.rom.NintendoDSRom.fromFile('nsmb.nds')



    print(len(rom.files))



.. code-block:: text



    2088



Или посмотреть, по какому адресу войдёт основной ARM9-код:



.. code-block:: python

    :emphasize-lines: 5

    :linenos:



    import ndspy.rom



    rom = ndspy.rom.NintendoDSRom.fromFile('nsmb.nds')



    print(hex(rom.arm9RamAddress))



.. code-block:: text



    0x2000000



Но, конечно, главное — извлечь файл.





Извлечение файла

----------------



Я собираюсь вытащить ``polygon_unit/evf_cloud1.nsbtx``, текстуру облаков переднего

плана в World 7-1.



.. figure:: images/rom-before.png

    :scale: 30%

    :align: center



    Как выглядит World 7-1 в обычном *New Super Mario Bros.* — на скриншоте

    видны полтора облака переднего плана.



Прежде чем продолжить, нужно понять, как относятся друг к другу файлы,

имена и ID в ROM. Это объясняется во введении к модулю :py:mod:`ndspy.fnt`

(используемому внутри :py:mod:`ndspy.rom`): :ref:`file-names-and-file-ids`.

Но главное — файлы обращаются по ID, а ID — это индексы в общем списке файлов.

Таблицы имён существуют отдельно, просто сопоставляя имена (и папки) с ID.



Найти ID ``polygon_unit/evf_cloud1.nsbtx`` можно через таблицу

:py:class:`ndspy.fnt.Folder`, которая хранится в атрибуте ``.filenames`` объекта

ROM. Распечатайте её, чтобы увидеть все имена и соответствующие ID (предупреждение:

вывод длинный):



.. code-block:: python

    :emphasize-lines: 5

    :linenos:



    import ndspy.rom



    rom = ndspy.rom.NintendoDSRom.fromFile('nsmb.nds')



    print(rom.filenames)



.. code-block:: text



    0131 00DUMMY

    0132 BUILDTIME

    0133 mgvs_sound_data.sdat

    0134 sound_data.sdat

    0135 ARCHIVE/

    0135     ARC0.narc

    0136     bomthrow.narc

    0137     card.narc

      [snip]

    1896     pl_ttl_LZ.bin

    1897     plnovs_LZ.bin

    1898 polygon_unit/

    1898     evf_cloud1.nsbtx

    1899     evf_haze1.nsbtx

    1900     evf_sea1_a.nsbtx

      [snip]

    2085     UI_O_menu_title_logo_o_u_ncg.bin

    2086     UI_O_menu_title_logo_u.bncl

    2087     UI_O_menu_title_o_d_ncg.bin



Отсюда видно: ID ``polygon_unit/evf_cloud1.nsbtx`` равен 1898. Как получить

это программно? Очень просто:



.. code-block:: python

    :emphasize-lines: 5

    :linenos:



    import ndspy.rom



    rom = ndspy.rom.NintendoDSRom.fromFile('nsmb.nds')



    print(rom.filenames.idOf('polygon_unit/evf_cloud1.nsbtx'))



.. code-block:: text



    1898



Или короче: :py:class:`ndspy.fnt.Folder` поддерживает индексацию,

позволяющую переходить от имён к ID:



.. code-block:: python

    :emphasize-lines: 5

    :linenos:



    import ndspy.rom



    rom = ndspy.rom.NintendoDSRom.fromFile('nsmb.nds')



    print(rom.filenames['polygon_unit/evf_cloud1.nsbtx'])



.. code-block:: text



    1898



Теперь можно взять данные файла, обратившись к списку ``.files`` по этому ID:



.. code-block:: python

    :emphasize-lines: 5-8

    :linenos:



    import ndspy.rom



    rom = ndspy.rom.NintendoDSRom.fromFile('nsmb.nds')



    cloudNSBTXFileID = rom.filenames['polygon_unit/evf_cloud1.nsbtx']

    cloudNSBTX = rom.files[cloudNSBTXFileID]



    print(cloudNSBTX[:50])



.. code-block:: text



    bytearray(b'BTX0\xff\xfe\x01\x00\x84a\x00\x00\x10\x00\x01\x00\x14\x00\x00\x00TEX0pa\x00\x00\x00\x00\x00\x00\x00\x00<\x00\x00\x00\x00\x00\x90\x00\x00\x00\x00\x00\x00\x00\x00\x08')



Супер.



.. note::



    Возможно, вы, как и я, задумываетесь, зачем мы получили ``bytearray``, а не

    :py:class:`bytes`. ``bytearray`` — изменяемая версия :py:class:`bytes`, и

    ndspy отдаёт данные в таком виде, чтобы было удобно их править.



Поскольку часто нужна именно полученная по имени запись, есть ещё один

сокращённый метод:



.. code-block:: python

    :emphasize-lines: 5

    :linenos:



    import ndspy.rom



    rom = ndspy.rom.NintendoDSRom.fromFile('nsmb.nds')



    cloudNSBTX = rom.getFileByName('polygon_unit/evf_cloud1.nsbtx')



    print(cloudNSBTX[:50])



.. code-block:: text



    bytearray(b'BTX0\xff\xfe\x01\x00\x84a\x00\x00\x10\x00\x01\x00\x14\x00\x00\x00TEX0pa\x00\x00\x00\x00\x00\x00\x00\x00<\x00\x00\x00\x00\x00\x90\x00\x00\x00\x00\x00\x00\x00\x00\x08')



.. note::



    Возможно, вам хочется сразу переходить к этой короткой записи. Но важно

    понимать, от чего именно она упрощает работу — ведь не всегда вы сможете

    воспользоваться сокращением. Например, методы ``.fromFile(filename)``

    не пригодятся, если вы хотите загрузить файл, уже прочитанный из ROM:

    ROM-файлы поставляются как :py:class:`bytes`, поэтому удобнее использовать

    конструкторы, принимающие байты напрямую.



Теперь можно сохранить ``evf_cloud1.nsbtx`` наружу:



.. code-block:: python

    :emphasize-lines: 7-8

    :linenos:



    import ndspy.rom



    rom = ndspy.rom.NintendoDSRom.fromFile('nsmb.nds')



    cloudNSBTX = rom.getFileByName('polygon_unit/evf_cloud1.nsbtx')



    with open('evf_cloud1.nsbtx', 'wb') as f:

        f.write(cloudNSBTX)



Теперь можно открыть файл в стороннем инструменте (например,

`MKDS Course Modifier <https://www.romhacking.net/utilities/1285/>`_) и внести

изменения.



Делайте это прямо сейчас — я подожду.





Замена файла

------------



Готово? Тогда пора заменить файл в ROM на новую версию.



Предположим, вы сохранили изменённый NSBTX как ``evf_cloud1_edited.nsbtx``.

Нам надо получить байты этого файла и положить их в ``rom.files``. Сначала

читаем файл:



.. code-block:: python

    :emphasize-lines: 5-6

    :linenos:



    import ndspy.rom



    rom = ndspy.rom.NintendoDSRom.fromFile('nsmb.nds')



    with open('evf_cloud1_edited.nsbtx', 'rb') as f:

        cloudNSBTXEdited = f.read()



Теперь можно записать эти байты по нужному ID:



.. code-block:: python

    :emphasize-lines: 8

    :linenos:



    import ndspy.rom



    rom = ndspy.rom.NintendoDSRom.fromFile('nsmb.nds')



    with open('evf_cloud1_edited.nsbtx', 'rb') as f:

        cloudNSBTXEdited = f.read()



    rom.files[rom.filenames['polygon_unit/evf_cloud1.nsbtx']] = cloudNSBTXEdited



Или, используя удобную функцию ndspy:



.. code-block:: python

    :emphasize-lines: 8

    :linenos:



    import ndspy.rom



    rom = ndspy.rom.NintendoDSRom.fromFile('nsmb.nds')



    with open('evf_cloud1_edited.nsbtx', 'rb') as f:

        cloudNSBTXEdited = f.read()



    rom.setFileByName('polygon_unit/evf_cloud1.nsbtx', cloudNSBTXEdited)



Готово. Осталось сохранить изменённый ROM и проверить!





Сохранение ROM

-------------



:py:class:`NintendoDSRom` предоставляет метод ``.save()``, возвращающий

:py:class:`bytes`, которые можно записать:



.. code-block:: python

    :emphasize-lines: 10-11

    :linenos:



    import ndspy.rom



    rom = ndspy.rom.NintendoDSRom.fromFile('nsmb.nds')



    with open('evf_cloud1_edited.nsbtx', 'rb') as f:

        cloudNSBTXEdited = f.read()



    rom.setFileByName('polygon_unit/evf_cloud1.nsbtx', cloudNSBTXEdited)



    with open('nsmb_edited.nds', 'wb') as f:

        f.write(rom.save())



Разумеется, и на это есть сокращение:



.. code-block:: python

    :emphasize-lines: 10

    :linenos:



    import ndspy.rom



    rom = ndspy.rom.NintendoDSRom.fromFile('nsmb.nds')



    with open('evf_cloud1_edited.nsbtx', 'rb') as f:

        cloudNSBTXEdited = f.read()



    rom.setFileByName('polygon_unit/evf_cloud1.nsbtx', cloudNSBTXEdited)



    rom.saveToFile('nsmb_edited.nds')



Вот и всё! Запустите ROM и посмотрите, что получилось.



.. figure:: images/rom-after.png

    :scale: 30%

    :align: center



    Отлично.
