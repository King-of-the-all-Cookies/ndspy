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

Журнал изменений
================

На этой странице приводится полный список изменений для всех релизов ndspy.
Все старые версии доступны для скачивания на `странице Releases на GitHub
<https://github.com/RoadrunnerWMC/ndspy/releases>`_.

.. contents:: :local:

4.2.0 (13 сентября 2024)
-----------------------

*   Добавлены аннотации типов для модулей :py:mod:`ndspy.bmg`, :py:mod:`ndspy.code`, :py:mod:`ndspy.codeCompression`, :py:mod:`ndspy.fnt`, :py:mod:`ndspy.lz10`, :py:mod:`ndspy.narc` и :py:mod:`ndspy.rom`. Эти аннотации помогают анализаторам типов проверять ваш код и улучшают подсказки в некоторых IDE. Спасибо mike8699 за помощь!
*   Небольшие улучшения документации, сообщений об ошибках и инфраструктуры тестирования.

4.1.0 (28 июля 2023)
--------------------

*   Заменена зависимость `crcmod` на чисто Python-реализацию CRC16, встроенную прямо в ndspy.
*   Исправлена ошибка в недокументированном модуле :py:mod:`ndspy.graphics2D`.
*   Пара улучшений в документации.

4.0.0 (15 марта 2022)
--------------------

*   Множество исправлений. Спасибо всем, кто сообщал об ошибках!
*   :py:mod:`ndspy.codeCompression` и :py:mod:`ndspy.lz10` теперь тоже имеют CLI. Плюс появились вспомогательные функции для сжатия и разжатия из/в файлы, а не только в :py:class:`bytes`.
*   :py:class:`ndspy.bmg.BMG` теперь использует значение кодировки 1 ``'cp1252'`` вместо предыдущего ``'latin-1'`` (латиница была лишь предположением автора). Также добавлен новый атрибут только для чтения :py:class:`ndspy.bmg.BMG.fullEncoding`, полезный при ручной декодировке строк BMG.
*   :py:attr:`ndspy.rom.NintendoDSRom.iconBanner` теперь поддерживает все вариации данных icon/banner, а не только первую версию. Константа ``ICON_BANNER_LEN`` удалена, потому что она не отражает реальной длины (разные версии имеют разные размеры).
*   :py:class:`ndspy.Processor` теперь :py:class:`enum.IntEnum`, а не просто :py:class:`enum.Enum`.
*   Сообщения assert теперь содержат текст, поясняющий, что пошло не так.
*   Модули :py:mod:`ndspy` и :py:mod:`ndspy.bmg` получили модульные тесты.
*   Изменения, касающиеся недокументированных модулей:

    *    API :py:mod:`ndspy.color` переработан. Возможно, его придётся вернуть назад или снова изменить до стабилизации модуля.
    *    Почти все перечисления :py:class:`ndspy.texture.TextureFormat` переименованы.
    *    :py:mod:`ndspy.graphics2D` получил дополнительные улучшения API.
    *    :py:mod:`ndspy.extras.music` теперь автоматически парсит ещё не разобранные *SSEQ*.

3.0.0 (10 февраля 2019)
----------------------

*   Полностью переработан API :py:mod:`ndspy.narc` ради совместимости с *New Super Mario Bros.* Это серьёзное несовместимое изменение, так что код, использующий модуль, однозначно требует обновления.
*   Серьёзные перемены в API :py:mod:`ndspy.bmg` ради совместимости, по сути, со всеми играми кроме *The Legend of Zelda: Phantom Hourglass* и *The Legend of Zelda: Spirit Tracks.* Это тоже несовместимое изменение, но в зависимости от того, какие части модуля использует ваш код, он всё ещё может работать без изменений.
*   Имена членов :py:class:`ndspy.soundSequence.MonoPolySequenceEvent.Value` и :py:class:`ndspy.soundSequence.VibratoTypeSequenceEvent.Value` приведены к верхнему регистру, как рекомендует стиль для enum.
*   В документацию добавлены первые два руководства и примерный код для некоторых модулей.
*   Перестроена структура папок документации. К сожалению, это делало недействительными большинство старых ссылок, но всё делалось с прицелом на то, чтобы в будущем это происходило как можно реже.
*   Изменения, касающиеся недокументированных модулей:

    *    Добавлены модули :py:mod:`ndspy.bnbl` и :py:mod:`ndspy.bncl`.
    *    :py:mod:`ndspy.graphics2D` получил ряд улучшений API.
    *    Изменено толкование значения альфы в :py:mod:`ndspy.color`.
    *    Добавлена возможность рендерить текстуры через :py:mod:`ndspy.texture`.

2.0.0 (23 января 2019)
----------------------

*   API :py:mod:`ndspy.soundBank` обновлён после обнаружения, что значения типов определения нот определены для всех типов инструментов, а не только для однонотных. (Спасибо, Gota7!) Это несовместимое изменение, поэтому у новой версии номер большой.
*   Устранены некоторые ошибки в :py:mod:`ndspy.soundBank` и :py:mod:`ndspy.soundSequence`, приводившие к крашам в определённых случаях. Если ваш код не падал на версии 1.0.x, это изменение вас не коснулось.
*   Добавлен :py:data:`ndspy.VERSION`.
*   Эта страница журнала изменений добавлена в документацию.

1.0.1 (18 января 2019)
----------------------

*   Исправлена проблема: pip пытался установиться на неподдерживаемые версии Python, вместо того чтобы выдать корректное сообщение об ошибке.

1.0.0 (18 января 2019)
----------------------

*   Первый релиз! API сильно менялся в неделях перед ним, так что если у вас есть код для ndspy до версии 1.0.0, скорее всего, придётся его адаптировать.

.. note::

    Этот релиз был удалён с PyPI из-за бага, исправленного в 1.0.1. Если он вам крайне необходим, скачайте его `с GitHub
    <https://github.com/RoadrunnerWMC/ndspy/releases/tag/v1.0.0>`_.
