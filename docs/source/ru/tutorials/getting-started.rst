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



.. title:: Первые шаги

РЈСЂРѕРє 0: РЅР°С‡Р°Р»Рѕ СЂР°Р±РѕС‚С‹

=====================



Р­С‚РѕС‚ В«СѓСЂРѕРєВ» РїСЂРѕСЃС‚Рѕ РїРѕРјРѕРіР°РµС‚ РІР°Рј РїРѕРґРіРѕС‚РѕРІРёС‚СЊСЃСЏ Рє РѕСЃС‚Р°Р»СЊРЅС‹Рј СЂСѓРєРѕРІРѕРґСЃС‚РІР°Рј.



РџСЂРµР¶РґРµ РІСЃРµРіРѕ: РµСЃР»Рё Сѓ РІР°СЃ РµС‰С‘ РЅРµ СѓСЃС‚Р°РЅРѕРІР»РµРЅ Python 3.6 РёР»Рё РЅРѕРІРµРµ Рё ndspy,

РѕР±СЂР°С‚РёС‚РµСЃСЊ Рє СЂР°Р·РґРµР»Сѓ :ref:`installation-ru`.



РќР°РїРёСЃР°РЅРёРµ С‚РµСЃС‚РѕРІРѕРіРѕ СЃРєСЂРёРїС‚Р°

---------------------------



Р СѓРєРѕРІРѕРґСЃС‚РІР° РЅРµ РїСЂРµРґРїРѕР»Р°РіР°СЋС‚ СЂР°Р±РѕС‚Сѓ РІ РёРЅС‚РµСЂР°РєС‚РёРІРЅРѕР№ РѕР±РѕР»РѕС‡РєРµ Python. Р’РјРµСЃС‚Рѕ

СЌС‚РѕРіРѕ РјС‹ РїРѕРєР°Р·С‹РІР°РµРј, РєР°Рє РїРёСЃР°С‚СЊ РєРѕСЂРѕС‚РєРёРµ СЃРєСЂРёРїС‚С‹ вЂ” СЌС‚Рѕ РјРѕР№ РїСЂРµРґРїРѕС‡РёС‚Р°РµРјС‹Р№

СЃРїРѕСЃРѕР± РІР·Р°РёРјРѕРґРµР№СЃС‚РІРёСЏ СЃ ndspy. РџРѕСЌС‚РѕРјСѓ СЃС‚РѕРёС‚ СѓР±РµРґРёС‚СЊСЃСЏ, С‡С‚Рѕ РІС‹ СѓРјРµРµС‚Рµ СЌС‚Рѕ

РґРµР»Р°С‚СЊ.



РЎРѕР·РґР°Р№С‚Рµ РЅРѕРІС‹Р№ Python-С„Р°Р№Р» (РЅР°РїСЂРёРјРµСЂ, ``test.py``), РѕС‚РєСЂРѕР№С‚Рµ РµРіРѕ РІ Р»СЋР±РёРјРѕРј

СЂРµРґР°РєС‚РѕСЂРµ Рё РІСЃС‚Р°РІСЊС‚Рµ СЃР»РµРґСѓСЋС‰РµРµ:



.. code-block:: python

    :linenos:



    import ndspy



    print('Hello world!')



Р—Р°С‚РµРј РїРѕРїСЂРѕР±СѓР№С‚Рµ Р·Р°РїСѓСЃС‚РёС‚СЊ СЃРєСЂРёРїС‚ С‚РѕР№ РєРѕРїРёРµР№ Python, РІ РєРѕС‚РѕСЂСѓСЋ РІС‹ СѓСЃС‚Р°РЅРѕРІРёР»Рё

ndspy. Р’Р°С€ СЂРµРґР°РєС‚РѕСЂ РјРѕР¶РµС‚ РїСЂРµРґРѕСЃС‚Р°РІРёС‚СЊ РІСЃС‚СЂРѕРµРЅРЅС‹Р№ СЃРїРѕСЃРѕР± Р·Р°РїСѓСЃРєР°, Р»РёР±Рѕ РЅР°РґРѕ

РІС‹РїРѕР»РЅРёС‚СЊ РєРѕРјР°РЅРґСѓ РёР· С‚РµСЂРјРёРЅР°Р»Р° / РєРѕРјР°РЅРґРЅРѕР№ СЃС‚СЂРѕРєРё, РЅР°РїСЂРёРјРµСЂ:



.. code-block:: text



    python3 test.py



    py -3 -m test.py



Р•СЃР»Рё СЃРєСЂРёРїС‚ РІС‹РІРѕРґРёС‚ ``Hello world!`` Рё Р·Р°РІРµСЂС€Р°РµС‚СЃСЏ, РІСЃС‘ СЂР°Р±РѕС‚Р°РµС‚.



.. tip::



    Р•СЃР»Рё РІС‹ РїРѕР»СѓС‡РёС‚Рµ С‚Р°РєСѓСЋ РѕС€РёР±РєСѓ:



    .. code-block:: text



        Traceback (most recent call last):

          File "test.py", line 1, in <module>

            import ndspy

        ModuleNotFoundError: No module named 'ndspy'



    Р­С‚Рѕ РѕР·РЅР°С‡Р°РµС‚, С‡С‚Рѕ РІС‹ Р·Р°РїСѓСЃРєР°РµС‚Рµ С‚Сѓ РєРѕРїРёСЋ Python, РІ РєРѕС‚РѕСЂРѕР№ ndspy РЅРµ СѓСЃС‚Р°РЅРѕРІР»РµРЅ.

    РџРѕРїСЂРѕР±СѓР№С‚Рµ РІРјРµСЃС‚Рѕ РїСЂРѕСЃС‚Рѕ ``3`` СѓРєР°Р·Р°С‚СЊ РґРІРµ РїРµСЂРІС‹Рµ С†РёС„СЂС‹ РІРµСЂСЃРёРё, РєРѕС‚РѕСЂСѓСЋ РІС‹

    СѓСЃС‚Р°РЅРѕРІРёР»Рё, РЅР°РїСЂРёРјРµСЂ ``python3.6 test.py``.
