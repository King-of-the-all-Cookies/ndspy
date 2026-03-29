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



``ndspy.codeCompression``: СЃР¶Р°С‚РёРµ РєРѕРґР°

======================================



РљРѕРјР°РЅРґРЅС‹Р№ РёРЅС‚РµСЂС„РµР№СЃ :py:mod:`ndspy.codeCompression` РїРѕР·РІРѕР»СЏРµС‚ Р»РµРіРєРѕ СЃР¶РёРјР°С‚СЊ Рё

СЂР°Р·Р¶РёРјР°С‚СЊ С„Р°Р№Р»С‹ РІ С„РѕСЂРјР°С‚Рµ, РёСЃРїРѕР»СЊР·СѓРµРјРѕРј РґР»СЏ РёСЃРїРѕР»РЅСЏРµРјРѕРіРѕ РєРѕРґР° NDS. Р’С‹ РјРѕР¶РµС‚Рµ

Р·Р°РїСѓСЃРєР°С‚СЊ РµРіРѕ С‡РµСЂРµР· ``python3 -m ndspy.codeCompression`` РёР»Рё

``ndspy_codeCompression`` Р»РёР±Рѕ РїСЂРѕРіСЂР°РјРјРЅРѕ, РІС‹Р·РІР°РІ :py:func:`ndspy.codeCompression.main`.



РљСЂР°С‚РєРѕРµ РѕРїРёСЃР°РЅРёРµ РёСЃРїРѕР»СЊР·РѕРІР°РЅРёСЏ:



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



РњРѕРґСѓР»СЊ РїСЂРµРґРѕСЃС‚Р°РІР»СЏРµС‚ РєРѕРјР°РЅРґС‹ РґР»СЏ СЃР¶Р°С‚РёСЏ Рё СЂР°Р·Р¶Р°С‚РёСЏ:



РЎР¶Р°С‚РёРµ (``compress`` / ``c``)

----------------------------



РљСЂР°С‚РєРѕРµ РѕРїРёСЃР°РЅРёРµ:



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



Р­С‚Р° РєРѕРјР°РЅРґР° СЃР¶РёРјР°РµС‚ С„Р°Р№Р» РІ С„РѕСЂРјР°С‚Рµ СЃР¶Р°С‚РёСЏ РєРѕРґР°. Р•СЃР»Рё РёРјСЏ РІС‹С…РѕРґРЅРѕРіРѕ С„Р°Р№Р»Р° РЅРµ

СѓРєР°Р·Р°РЅРѕ, РёСЃРїРѕР»СЊР·СѓРµС‚СЃСЏ РёРјСЏ РІС…РѕРґРЅРѕРіРѕ СЃ РґРѕР±Р°РІР»РµРЅРёРµРј СЂР°СЃС€РёСЂРµРЅРёСЏ ``.cmp``.



РђСЂРіСѓРјРµРЅС‚ ``--is_arm9`` СЃРѕРѕС‚РІРµС‚СЃС‚РІСѓРµС‚ РїР°СЂР°РјРµС‚СЂСѓ ``isArm9`` С„СѓРЅРєС†РёРё

:py:func:`compress() <ndspy.codeCompression.compress>`.



Р Р°Р·Р¶Р°С‚РёРµ (``decompress`` / ``d``)

--------------------------------



РљСЂР°С‚РєРѕРµ РѕРїРёСЃР°РЅРёРµ:



.. code-block:: text



    $ python3 -m ndspy.codeCompression decompress -h

    usage: codeCompression.py decompress [-h] input_file [output_file]



    positional arguments:

      input_file   input file to decompress

      output_file  what to save the decompressed file as



    optional arguments:

      -h, --help   show this help message and exit



Р­С‚Р° РєРѕРјР°РЅРґР° СЂР°Р·Р¶РёРјР°РµС‚ С„Р°Р№Р», СЃР¶Р°С‚С‹Р№ РІ С„РѕСЂРјР°С‚Рµ РєРѕРґР°. Р•СЃР»Рё РёРјСЏ РІС‹С…РѕРґРЅРѕРіРѕ С„Р°Р№Р»Р°

РЅРµ Р·Р°РґР°РЅРѕ, РёСЃРїРѕР»СЊР·СѓРµС‚СЃСЏ РёРјСЏ РІС…РѕРґРЅРѕРіРѕ С„Р°Р№Р»Р° СЃ СЂР°СЃС€РёСЂРµРЅРёРµРј ``.dec``.
