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



``ndspy.codeCompression``: сжатие кода

=====================================



.. py:module:: ndspy.codeCompression
    :noindex:




Модуль :py:mod:`ndspy.codeCompression` предоставляет функции для работы с

форматом сжатия, применяемым к исполняемым код-файлам.



Этот формат иногда называют «BLZ» (reverse LZ). По сути, он похож на LZ10,

но разжатие идёт в обратном направлении — с конца файла. Такое поведение

позволяет распаковывать данные прямо в памяти консоли.



.. note::

    Если вы работаете через :py:mod:`ndspy.code`, сжатие уже выполняется за вас,

    и обращаться к этому модулю вручную обычно не нужно.



.. seealso::



    В модуле также есть :doc:`интерфейс командной строки

    <../cli/codeCompression>`.





.. py:function:: compress(data[, isArm9=False])
    :noindex:




    Compress code data. This is the inverse of :py:func:`decompress`.



    :param bytes data: The data to compress.



    :param bool isArm9: Whether the data to be compressed is a main ARM9 code

        file or not. ARM9 code needs to be compressed slightly differently.

        (This should be ``False`` for overlays.)



        :default: ``False``



    :returns: The compressed data.

    :rtype: :py:class:`bytes`





.. py:function:: compressFromFile(filePath[, isArm9=False])
    :noindex:




    Load a filesystem file, and compress its contents. This is the inverse of

    :py:func:`decompressToFile`, and is a convenience function.



    :param filePath: The path to the file to open.

    :type filePath: :py:class:`str` or other path-like object



    :param bool isArm9: Equivalent to the same parameter in the

        :py:func:`compress` function.



    :returns: The compressed data.

    :rtype: :py:class:`bytes`





.. py:function:: compressToFile(data, filePath[, isArm9=False])
    :noindex:




    Compress data in the code compression format, and save it to a filesystem

    file. This is the inverse of :py:func:`decompressFromFile`, and is a

    convenience function.



    :param bytes data: The data to compress.



    :param filePath: The path to the compressed file to save to.

    :type filePath: :py:class:`str` or other path-like object



    :param bool isArm9: Equivalent to the same parameter in the

        :py:func:`compress` function.





.. py:function:: decompress(data)
    :noindex:




    Decompress data that was compressed using code compression. This is the

    inverse of :py:func:`compress`.



    If the data does not seem to be compressed, it will be returned unmodified.



    :param bytes data: The compressed data.



    :returns: The decompressed data.

    :rtype: :py:class:`bytes`





.. py:function:: decompressFromFile(filePath)
    :noindex:




    Load a filesystem file that is compressed using code compression, and

    decompress it. This is the inverse of :py:func:`compressToFile`, and is a

    convenience function.



    :param filePath: The path to the compressed file to open.

    :type filePath: :py:class:`str` or other path-like object



    :returns: The decompressed data.

    :rtype: :py:class:`bytes`





.. py:function:: decompressToFile(data, filePath)
    :noindex:




    Decompress data that was compressed using code compression, and save it to

    a filesystem file. This is the inverse of :py:func:`compressFromFile`, and

    is a convenience function.



    :param bytes data: The data to decompress.



    :param filePath: The path to the file to save to.

    :type filePath: :py:class:`str` or other path-like object





.. py:function:: main([args])
    :noindex:




    This is the main function for :doc:`this module's command-line interface

    <../cli/codeCompression>`. This allows you to invoke the CLI

    programmatically, if you would like.



    :param args: The command-line arguments. Defaults to ``sys.argv`` if not

        provided.

    :type args: :py:class:`list` of :py:class:`str`
