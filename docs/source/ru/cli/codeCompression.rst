..
    Copyright 2020 RoadrunnerWMC

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

``ndspy.codeCompression``: сжатие кода
======================================

Командный интерфейс :py:mod:`ndspy.codeCompression` позволяет легко сжимать и
разжимать файлы в формате, используемом для исполняемого кода NDS. Вы можете
запускать его через ``python3 -m ndspy.codeCompression`` или
``ndspy_codeCompression`` либо программно, вызвав :py:func:`ndspy.codeCompression.main`.

Краткое описание использования:

.. code-block:: text

    $ python3 -m ndspy.codeCompression -h
    usage: codeCompression.py [-h] {compress,c,decompress,d} ...

    ndspy.codeCompression CLI: Compress or decompress files using the code
    compression format.

    optional arguments:
      -h, --help            show this help message and exit

    commands:
      (run a command with -h for additional help)

      {compress,c,decompress,d}
        compress (c)        compress a file
        decompress (d)      decompress a file

Модуль предоставляет команды для сжатия и разжатия:

Сжатие (``compress`` / ``c``)
----------------------------

Краткое описание:

.. code-block:: text

    $ python3 -m ndspy.codeCompression compress -h
    usage: codeCompression.py compress [-h] [--is_arm9] input_file [output_file]

    positional arguments:
      input_file   input file to compress
      output_file  what to save the compressed file as

    optional arguments:
      -h, --help   show this help message and exit
      --is_arm9    treat the data as a main ARM9 code file (do not use for
                   overlays)

Эта команда сжимает файл в формате сжатия кода. Если имя выходного файла не
указано, используется имя входного с добавлением расширения ``.cmp``.

Аргумент ``--is_arm9`` соответствует параметру ``isArm9`` функции
:py:func:`compress() <ndspy.codeCompression.compress>`.

Разжатие (``decompress`` / ``d``)
--------------------------------

Краткое описание:

.. code-block:: text

    $ python3 -m ndspy.codeCompression decompress -h
    usage: codeCompression.py decompress [-h] input_file [output_file]

    positional arguments:
      input_file   input file to decompress
      output_file  what to save the decompressed file as

    optional arguments:
      -h, --help   show this help message and exit

Эта команда разжимает файл, сжатый в формате кода. Если имя выходного файла
не задано, используется имя входного файла с расширением ``.dec``.
