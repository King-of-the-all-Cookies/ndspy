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

ndspy
=====

.. image:: https://img.shields.io/discord/534221996230180884.svg?logo=discord&logoColor=white&colorB=7289da
    :alt: Discord
    :target: https://discord.gg/RQhxAxw

.. image:: https://img.shields.io/badge/repo-GitHub-brightgreen.svg?logo=github&logoColor=white
    :alt: GitHub
    :target: https://github.com/RoadrunnerWMC/ndspy

.. image:: https://img.shields.io/pypi/v/ndspy.svg?logo=python&logoColor=white
    :alt: PyPI
    :target: https://pypi.org/project/ndspy/

.. image:: https://img.shields.io/github/license/RoadrunnerWMC/ndspy.svg?logo=gnu&logoColor=white
    :alt: License: GNU GPL 3.0
    :target: https://www.gnu.org/licenses/gpl-3.0

**ndspy** («эн-ди-ЭС-пай») — это библиотека на Python и набор утилит командной строки, которые помогают читать, изменять и создавать множество типов файлов, используемых в играх для Nintendo DS.

ndspy придерживается нескольких ключевых принципов проектирования:

-   **Точность**: ndspy должна уметь открывать и заново сохранять любой поддерживаемый файл с послеточным совпадением, если он находится в своем каноническом формате [#canonical-format]_.
-   **Гибкость**: ndspy должна читать любой корректный файл в поддерживаемом формате. Даже если есть высокая вероятность того, что какая-то особенно сложная часть файла не удастся полностью интерпретировать, библиотека по-прежнему должна быть полезна для редактирования остальных частей.
-   **Семантичность**: API ndspy должны максимально соответствовать семантике файловых структур, скрывая двоичные детали.

ndspy предоставляет как Python-API, так и набор простых консольных утилит, которые его используют. Утилиты позволяют преобразовывать файлы в бинарный формат и обратно без написания собственного Python-кода [#cli-tools]_. API пригодны для использования в Python-приложениях и скриптах, когда нужно сделать что-то более сложное, чем позволяют утилиты.

Так как ndspy написан на чистом Python, он кроссплатформенный и работает на любых системах, поддерживаемых Python. Учтите, что Python сам по себе не работает на Nintendo DS; ndspy предназначен для использования на вашем компьютере.

Хочется попробовать? Прочитайте дальше, чтобы увидеть примеры, или загляните в :doc:`api/index`, чтобы изучить документацию конкретного модуля. Когда будете готовы устанавливать, перейдите к разделу :ref:`installation-ru`.

.. note::
    Если вы собираетесь использовать ndspy для работы со звуковыми данными и ещё не знакомы с файлами *SDAT*, сначала прочитайте :doc:`приложение, объясняющее их структуру <appendices/sdat-structure>`.

Несколько примеров работы ndspy
-------------------------------

.. testsetup:: *

    import os, os.path
    import shutil
    import tempfile

    origCwd = os.getcwd()
    dir = tempfile.TemporaryDirectory()
    os.chdir(dir.name)

    if haveNSMB:
        shutil.copyfile(nsmbRomPath, 'nsmb.nds')

    shutil.copyfile(testFilesPath / 'never-gonna-give-you-up.sseq',
                    'never-gonna-give-you-up.sseq')

.. testcleanup:: *

    os.chdir(origCwd)
    dir.cleanup()

Создание файла *BMG* с текстовыми сообщениями:

.. doctest::

    >>> import ndspy.bmg
    >>> message1 = ndspy.bmg.Message(b'', ['Open your eyes...'])
    >>> message2 = ndspy.bmg.Message(b'', ['Wake up, Link...'])
    >>> bmg = ndspy.bmg.BMG.fromMessages([message1, message2])
    >>> bmg.save()
    b'MESGbmg1\xa0\x00\x00\x00\x02\x00\x00\x00\x02\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00INF1 \x00\x00\x00\x02\x00\x04\x00\x00\x00\x00\x00\x02\x00\x00\x00&\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00DAT1`\x00\x00\x00\x00\x00O\x00p\x00e\x00n\x00 \x00y\x00o\x00u\x00r\x00 \x00e\x00y\x00e\x00s\x00.\x00.\x00.\x00\x00\x00W\x00a\x00k\x00e\x00 \x00u\x00p\x00,\x00 \x00L\x00i\x00n\x00k\x00.\x00.\x00.\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00'
    >>>

Измените все ноты в файле *SSEQ*, что аналогично `этой песне <https://youtu.be/cSAp9sBzPbc>`_:

.. doctest::

    >>> import ndspy.soundSequence
    >>> song = ndspy.soundSequence.SSEQ.fromFile('never-gonna-give-you-up.sseq')
    >>> song.parse()
    >>> for event in song.events:
    ...     if isinstance(event, ndspy.soundSequence.NoteSequenceEvent):
    ...         event.pitch = 60
    ...
    >>> song.saveToFile('never-gonna-give-you-up-but-all-the-notes-are-c.sseq')
    >>>

Сжатие и разжатие данных с использованием формата *LZ10*:

.. doctest::

    >>> import ndspy.lz10
    >>> compressed = ndspy.lz10.compress(b'This is some data to compress')
    >>> compressed
    b'\x10\x1d\x00\x00\x04This \x00\x02so\x00me data \x00to compr\x00ess\x00\x00\x00\x00\x00'
    >>> ndspy.lz10.decompress(compressed)
    b'This is some data to compress'
    >>>

Поиск всех файлов, начинающихся с определенной последовательности байтов, внутри ROM:

.. doctest::
    :skipif: not haveNSMB

    >>> import ndspy.rom
    >>> rom = ndspy.rom.NintendoDSRom.fromFile('nsmb.nds')
    >>> for i, file in enumerate(rom.files):
    ...     if file.startswith(b'BMD0'):
    ...         print(rom.filenames[i] + ' is a NSBMD model')
    ...
    demo/end_kp.nsbmd is a NSBMD model
    demo/staffroll.nsbmd is a NSBMD model
    demo/staffroll_back.nsbmd is a NSBMD model
    enemy/A_jiku.nsbmd is a NSBMD model
    enemy/all_goal_flag.nsbmd is a NSBMD model
    ...
    map/world7.nsbmd is a NSBMD model
    map/world8.nsbmd is a NSBMD model
    >>>

Распространенные заблуждения
----------------------------

Все ещё не совсем ясно, что такое ndspy и на что он способен? Ниже — ответы на некоторые типичные вопросы.

-   ndspy — это *библиотека*, а не отдельная *программа*. Чтобы использовать ndspy, нужно писать собственный Python-код; ndspy просто предоставляет инструменты, которыми этот код может пользоваться. Возможно, сначала покажется сложно, особенно если вы не очень хорошо знакомы с Python, но :doc:`руководства <tutorials/index>` шаг за шагом ведут через процесс решения типовых задач. В будущем можно ожидать появления дополнительных консольных или даже графических утилит на базе ndspy, но пока работать приходится именно так.
-   ndspy выполняется на вашем компьютере, а не на самой Nintendo DS. С его помощью вы создаёте и редактируете файлы игры, которые затем запускаются на консоли. Игры для DS пишутся на компилируемых языках вроде C или C++, потому что только они позволяют добиться достаточной производительности; Python никогда не станет серьёзным вариантом для самой консоли.
-   ndspy не поддерживает абсолютно все форматы файлов всех игр для DS. Наоборот, в конкретной игре большинство файлов *не будут* поддерживаться ndspy. Существуют тысячи различных форматов, и поддерживать их все невозможно. ndspy сосредоточен на форматах, которые часто используются в играх, особенно фирменных. Форматы, специфичные для конкретной игры, лучше оформлять в отдельных библиотеках.

    Тем не менее некоторые части ndspy (например, работа с ROM и необработанными текстурными данными) относятся к аппаратной части консоли, а значит потенциально актуальны для большинства или всех игр.

.. _installation-ru:

Установка
---------

ndspy требует Python 3.6 или новее. Поддерживаются как CPython (референсная реализация), так и PyPy. Python 2 не поддерживается.

Самый простой способ получить последнюю стабильную версию ndspy — установить её через PyPI с помощью pip.

pip — это консольная программа, значит нужно запускать её из командной строки Windows или bash. Точная команда зависит от операционной системы и того, как вы устанавливали Python, но одна из этих должна подойти:

.. code-block:: text

    pip install ndspy

    python3 -m pip install ndspy

    py -3 -m pip install ndspy

Если вам нужна самая свежая версия ndspy с новыми функциями и исправлениями, можно скачать код из `репозитория GitHub <https://github.com/RoadrunnerWMC/ndspy>`_ и установить вручную.

Поддержка
----------

Я потратил много времени на написание документации ndspy, так что сначала убедитесь, что там уже нет ответа на ваш вопрос: проверьте :doc:`api/index` и :doc:`tutorials/index`.

Если это не помогло, задайте вопрос мне (RoadrunnerWMC) на `сервере Discord ndspy <https://discord.gg/RQhxAxw>`_. Я постараюсь ответить как можно быстрее!

Если вы думаете, что нашли баг в ndspy, пожалуйста, `создайте issue на GitHub <https://github.com/RoadrunnerWMC/ndspy/issues/new>`_. Спасибо!

Версионирование
---------------

ndspy по мере сил следует `семантическому версионированию <https://semver.org/>`_. Если какая-то утилита утверждает, что работает с ndspy 1.0.2, она должна работать и с ndspy 1.2.0, но не обязательно с ndspy 2.0.0 (просто имейте в виду, что некоторые из этих номеров версий вообще не существуют).

Недокументированные модули считаются исключением из семантического версионирования и могут изменяться в любой момент. Об этом также упоминается в разделе :ref:`undocumented-apis-ru`.

.. https://stackoverflow.com/a/16302843

.. toctree::
    :maxdepth: 2
    :caption: Содержание

    tutorials/index
    api/index
    cli/index
    changelog
    appendices/sdat-structure

.. _undocumented-apis-ru:

Недокументированные API
-----------------------

В ndspy иногда встречаются недокументированные модули Python. Их можно использовать, если хочется, но API таких модулей являются предварительными — то есть они могут измениться (или даже исчезнуть) в любой момент. Документированные модули, наоборот, следует считать стабильными и подчиняющимися семантическому версионированию.

Благодарности
-------------

**ndspy** написан RoadrunnerWMC на основе большого количества источников. В алфавитном порядке:

*   `Исходники apicula <https://github.com/scurest/apicula>`_ — отличный справочник по формату *NSBMD*.
*   `Custom Mario Kart Wiiki <http://wiki.tockdom.com/>`_ — сведения о версии *BMG*, используемой в играх Wii (отличающейся от той, что применяют игры DS, но схожей).
*   `DS Sound Studio <http://archive.dshack.org/thread.php?tid=2590>`_ — расшифровка значений проигрывателей последовательностей и потоков.
*   `Исходники DSDecmp <https://github.com/Barubary/dsdecmp>`_ (`страница <http://www.romhacking.net/utilities/789/>`_) — код для нескольких форматов сжатия.
*   `DSiBrew <http://www.dsibrew.org/>`_ — дополнительные сведения о формате ROM.
*   Личные переписки с Eugene#6990 в Discord — информация о PSG-инструментах в файлах *SBNK*.
*   `GBATEK <http://problemkaputt.de/gbatek.htm>`_ — разная техническая информация.
*   Личные переписки с Gota7#9350 в Discord — данные о типах определения нот в *SBNK*.
*   `Imran Nazar: The Smallest NDS File <http://imrannazar.com/The-Smallest-NDS-File>`_ — удобная шпаргалка по заголовку ROM и хороший тест для кода ROM-библиотеки.
*   `kiwi.ds Nitro Composer File (*.sdat) Specification <https://roadrunnerwmc.github.io/kiwids/sdat.html>`_ — один из лучших справочников по *SDAT*.
*   `Исходники melonDS <https://github.com/Arisotura/melonDS>`_ (`сайт <http://melonds.kuribo64.net/>`_) — особенно полезны куски, работающие с текстурами, но в целом отличный источник сведений о поведении железа.
*   `NDSTech Wiki (архив) <https://web.archive.org/web/20110106014930/http://www.bottledlight.com/ds/index.php/FileFormats/NDSFormat>`_ — дополнительная информация по формату ROM.
*   `Nintendo DS File Formats <http://www.romhacking.net/documents/[469]nds_formats.htm>`_ — отличный сборник спецификаций.
*   `Исходники Nintendo DS/GBA Compressors by CUE <http://www.romhacking.net/utilities/826/>`_ (`тред <https://gbatemp.net/threads/nintendo-ds-gba-compressors.313278/>`_) — код для обратного LZ-сжатия.
*   `Исходники NSMB Editor (NSMBe) <https://github.com/Dirbaio/NSMB-Editor>`_ — информация и код для множества форматов.
*   Личные переписки с Prof. 9 в Discord — сведения о файлах *SSEQ* и событиях последовательностей.
*   `Исходники sseq2mid <https://github.com/loveemu/loveemu-lab/tree/master/sseq2mid/src2>`_ — поддерживает больше типов событий, чем многие документы.
*   `Исходники Tinke <https://github.com/pleonex/tinke>`_ — заполняет пробелы по *SDAT*, где другие справочники неоднозначны.
*   Собственные исследования автора и `Skawo <https://www.youtube.com/user/skawo90>`_.

Спасибо всем авторам источников!

Индексы и таблицы
------------------

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`

.. todo::

    Было бы здорово добавить по примеру или два в начало каждого модуля.

    Функции для загрузки/сохранения BMG в/из `формата wbmgt
    <https://szs.wiimm.de/info/bmg-text.html>`_?

    Инструментам нужна серьёзная доработка и документация.

    Автоматизированное тестирование.

    Написать более подробные руководства.

    Попробовать запустить парсер SSEQ/SSAR на всех ROM-файлах, чтобы выявить проблемы.

    Зашифровка/расшифровка защищенной области ROM, перенесённая из ndstool.

    `https://en.wikipedia.org/wiki/Sample-based_synthesis#Multisampling
        Возможно, стоит переименовать региональные инструменты в соответствии с этой статьёй? И точно стоит дать на неё ссылку.

    Модульное тестирование:

        Чтобы протестировать класс, который парсит файл (встречается чаще всего), просто сделайте такие тесты:

        -   один файл, который содержит всё, что обычно содержит файл (например, без неиспользуемых инструментов SBNK);

        -   один пустой файл;

        -   как можно больше интересных пограничных случаев (например, неиспользуемые инструменты SBNK).

        Используйте ndspy, чтобы построить эти тестовые файлы.

        Дополнительно сохраните код, который использовался для построения файлов, и используйте его как второй набор тестов: действительно ли его запуск снова порождает ожидаемые файлы?
